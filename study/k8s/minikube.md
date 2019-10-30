# minikube

* Minikube v1.0.1/Kubernetes v1.14+
* 如需更新minikube，需要更新 minikube 安装包
  * `minikube delete` 删除现有虚机，删除 `~/.minikube` 目录缓存的文件

## 配置

### 先决条件

* 安装 [kubectl](https://kubernetes.io/docs/tasks/kubectl/install/)

**Mac OSX**

```text
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v1.0.1/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

**Linux**

```text
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v1.0.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

## 启动

缺省Minikube使用VirtualBox驱动来创建Kubernetes本地环境

```text
minikube start --registry-mirror=https://registry.docker-cn.com
```

支持不同的Kubernetes版本

```text
# 安装Kubernetes v1.12.1
minikube start --registry-mirror=https://registry.docker-cn.com --kubernetes-version v1.12.1
```

指定内存大小和CPU核数

```text
$ minikube start --memory=8192 --cpus=4 --kubernetes-version=v1.13.0 \
    --vm-driver=`your_vm_driver_choice`
```

```bash
minikube config set vm-driver virtualbox

minikube start --image-mirror-country cn \
    --iso-url=https://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/iso/minikube-v1.5.0.iso \
    --registry-mirror=https://1ob3wirb.mirror.aliyuncs.com
    --memory=8192 --cpus=4 --kubernetes-version=v1.16.0 
```

打开Kubernetes控制台

```text
minikube dashboard
```

访问

[http://127.0.0.1:57851/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/\#!/overview?namespace=istio-system](http://127.0.0.1:57851/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/#!/overview?namespace=istio-system)

网络打通

```text
$ minikube tunnel
```

