# Install Harbor

* 下载：从github上下载最新offline版本的[harbor](https://github.com/goharbor/harbor/releases)，下载后解压；
* 配置：进入解压目录，修改harbor.yml，主要是hostname、port和data目录；
* 安装docker和docker-compose
* 执行prepare，再执行install.sh
* 打开harbor控制，默认用户admin/Harbor12345
* 创建k8s项目和用户
* 在可以翻墙的电脑上docker pull k8s的镜像
* docker login 到harhor地址如reg.i3hh.com:8091 ，输入用户和密码
* 将刚才本地的k8s镜像docker push 到 harbor上；
* 在需要安装k8s的服务器上配置docker镜像地址，然后重启docker

```text
#centos8下 /etc/docker/daemon.json
{
    "registry-mirrors":["http://reg.i3hh.com:8091"],
    "insecure-registries":["http://reg.i3hh.com:8091"]
}

systemctl restart docker
```

* 从Harhor仓库拉取镜像

```bash
docker pull reg.i3hh.com:8091/k8s/kubernetesui/metrics-scraper
docker pull reg.i3hh.com:8091/k8s/kubernetesui/metrics-scraper:v1.0.2
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-proxy:v1.16.2
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-apiserver:v1.16.2
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-controller-manager:v1.16.2
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-scheduler:v1.16.2
docker pull reg.i3hh.com:8091/k8s/kubernetesui/dashboard:v2.0.0-beta5
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/etcd:3.3.15-0
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/coredns:1.6.2
docker pull reg.i3hh.com:8091/k8s/quay.io/coreos/flannel:v0.11.0-amd64
docker pull reg.i3hh.com:8091/k8s/k8s.gcr.io/pause:3.1
```

* 修改镜像tag

```bash

docker tag reg.i3hh.com:8091/k8s/kubernetesui/metrics-scraper:v1.0.2 kubernetesui/metrics-scraper:v1.0.2
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-proxy:v1.16.2 k8s.gcr.io/kube-proxy:v1.16.2
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-apiserver:v1.16.2 k8s.gcr.io/kube-apiserver:v1.16.2
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-controller-manager:v1.16.2 k8s.gcr.io/kube-controller-manager:v1.16.2
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-scheduler:v1.16.2 k8s.gcr.io/kube-scheduler:v1.16.2
docker tag reg.i3hh.com:8091/k8s/kubernetesui/dashboard:v2.0.0-beta5 kubernetesui/dashboard:v2.0.0-beta5
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/etcd:3.3.15-0 k8s.gcr.io/etcd:3.3.15-0
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/coredns:1.6.2 k8s.gcr.io/coredns:1.6.2
docker tag reg.i3hh.com:8091/k8s/quay.io/coreos/flannel:v0.11.0-amd64 quay.io/coreos/flannel:v0.11.0-amd64
docker tag reg.i3hh.com:8091/k8s/k8s.gcr.io/pause:3.1 k8s.gcr.io/pause:3.1
```

* 删除镜像tag

```bash
docker rmi -f reg.i3hh.com:8091/k8s/kubernetesui/metrics-scraper
docker rmi -f reg.i3hh.com:8091/k8s/kubernetesui/metrics-scraper:v1.0.2
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-proxy:v1.16.2
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-apiserver:v1.16.2
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-controller-manager:v1.16.2
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/kube-scheduler:v1.16.2
docker rmi -f reg.i3hh.com:8091/k8s/kubernetesui/dashboard:v2.0.0-beta5
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/etcd:3.3.15-0
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/coredns:1.6.2
docker rmi -f reg.i3hh.com:8091/k8s/quay.io/coreos/flannel:v0.11.0-amd64
docker rmi -f reg.i3hh.com:8091/k8s/k8s.gcr.io/pause:3.1
```

#### 另一方式

通过kubeadm的来imageRepository来切换拉取镜像库，首先调整一下push到harbor的镜像tag.

```text
docker tag k8s.gcr.io/kube-proxy:v1.16.2 reg.i3hh.com:8091/k8s/kube-proxy:v1.16.2 
docker tag k8s.gcr.io/kube-apiserver:v1.16.2 reg.i3hh.com:8091/k8s/kube-apiserver:v1.16.2 
docker tag k8s.gcr.io/kube-controller-manager:v1.16.2 reg.i3hh.com:8091/k8s/kube-controller-manager:v1.16.2 
docker tag k8s.gcr.io/kube-scheduler:v1.16.2 reg.i3hh.com:8091/k8s/kube-scheduler:v1.16.2 
docker tag k8s.gcr.io/etcd:3.3.15-0 reg.i3hh.com:8091/k8s/etcd:3.3.15-0 
docker tag k8s.gcr.io/coredns:1.6.2 reg.i3hh.com:8091/k8s/coredns:1.6.2 
docker tag k8s.gcr.io/pause:3.1 reg.i3hh.com:8091/k8s/pause:3.1 


docker push reg.i3hh.com:8091/k8s/kube-proxy:v1.16.2 
docker push reg.i3hh.com:8091/k8s/kube-apiserver:v1.16.2 
docker push reg.i3hh.com:8091/k8s/kube-controller-manager:v1.16.2 
docker push reg.i3hh.com:8091/k8s/kube-scheduler:v1.16.2 
docker push reg.i3hh.com:8091/k8s/etcd:3.3.15-0 
docker push reg.i3hh.com:8091/k8s/coredns:1.6.2 
docker push reg.i3hh.com:8091/k8s/pause:3.1 


docker rmi -f reg.i3hh.com:8091/k8s/kube-proxy:v1.16.2 
docker rmi -f reg.i3hh.com:8091/k8s/kube-apiserver:v1.16.2 
docker rmi -f reg.i3hh.com:8091/k8s/kube-controller-manager:v1.16.2 
docker rmi -f reg.i3hh.com:8091/k8s/kube-scheduler:v1.16.2 
docker rmi -f reg.i3hh.com:8091/k8s/etcd:3.3.15-0 
docker rmi -f reg.i3hh.com:8091/k8s/coredns:1.6.2 
docker rmi -f reg.i3hh.com:8091/k8s/pause:3.1 
```

* 配置kubeadm的config，如admin-config.yml

```text
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: v1.16.2
apiServer:
  extraArgs:
    advertise-address: 192.168.99.103
    anonymous-auth: "false"
    enable-admission-plugins: AlwaysPullImages,DefaultStorageClass
    audit-log-path: /root/k8s/audit.log
controlPlaneEndpoint: 192.168.99.103:6443
imageRepository: reg.i3hh.com:8091/k8s
networking:
  podSubnet: 10.244.0.0/16
```







