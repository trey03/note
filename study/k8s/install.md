###机器信息
```text
192.168.1.100   k8s-master
192.168.1.101   k8s-node-01
192.168.1.102   k8s-node-02
```

###准备信息
```shell 
# 所有机器
systemctl stop firewalld
systemctl disable firewalld

setenforce 0
sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config

# 关闭交换分区
swapoff -a
yes | cp /etc/fstab /etc/fstab_bak
cat /etc/fstab_bak |grep -v swap > /etc/fstab

# 设置网桥包经IPTables，core文件生成路径
echo """
vm.swappiness = 0
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
""" > /etc/sysctl.conf
sysctl -p
 
# 同步时间
yum install -y ntpdate
ntpdate -u ntp.api.bz

# 升级内核
参考：https://blog.csdn.net/qq_27281257/article/details/82049634

# 检查默认内核版本是否大于4.14，否则请调整默认启动参数
grub2-editenv list
 
#重启以更换内核
reboot

# 确认内核版本后，开启IPVS
uname -a

cat > /etc/sysconfig/modules/ipvs.modules <<EOF
#!/bin/bash
ipvs_modules="ip_vs ip_vs_lc ip_vs_wlc ip_vs_rr ip_vs_wrr ip_vs_lblc ip_vs_lblcr ip_vs_dh ip_vs_sh ip_vs_fo ip_vs_nq ip_vs_sed ip_vs_ftp nf_conntrack"
for kernel_module in \${ipvs_modules}; do
 /sbin/modinfo -F filename \${kernel_module} > /dev/null 2>&1
 if [ $? -eq 0 ]; then
 /sbin/modprobe \${kernel_module}
 fi
done
EOF

chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules && lsmod | grep ip_vs

# 所有主机：检查UUID和Mac
cat /sys/class/dmi/id/product_uuid
ip link

```

执行sysctl -p报错请参考centos7添加bridge-nf-call-ip6tables出现No such file or directory
```text
解决方法：
[root@localhost ~]# modprobe br_netfilter
[root@localhost ~]# ls /proc/sys/net/bridge
bridge-nf-call-arptables bridge-nf-filter-pppoe-tagged
bridge-nf-call-ip6tables bridge-nf-filter-vlan-tagged
bridge-nf-call-iptables bridge-nf-pass-vlan-input-dev
 
[root@localhost ~]# sysctl -p
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
```

###安装Docker
```shell
yum install -y docker-ce

# 编辑systemctl的Docker启动文件和配置文件
sed -i "13i ExecStartPost=/usr/sbin/iptables -P FORWARD ACCEPT" /usr/lib/systemd/system/docker.service
mkdir -p /etc/docker
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
  "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "registry-mirrors": ["https://1ob3wirb.mirror.aliyuncs.com"]
}
EOF
 
# 启动docker
systemctl daemon-reload
systemctl enable docker
systemctl start docker

# 导入本地下载好的k8s镜像包
docker load < k8s.tar

#安装kubeadm kubelet kubectl
yum install -y kubelet kubeadm kubectl

```


###启动服务
```shell

systemctl enable kubelet.service
systemctl start kubelet.service

kubeadm init --config kubeadm.yaml
kubectl apply -f kube-flannel.yml


#获取token
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret |grep 'dashboard-token' |awk '{print $1}')

#add work node
kubeadm join 192.168.1.102:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:01baf966bf3d781695e02ed36266db02fc7be0a989016c7ea6885b97e27b09ab

```

###网络路由
```shell
route add default gw 192.168.1.1
```