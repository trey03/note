- Minikube v1.0.1/Kubernetes v1.14+

- 如需更新minikube，需要更新 minikube 安装包
  - `minikube delete` 删除现有虚机，删除 `~/.minikube` 目录缓存的文件

### 配置

#### 先决条件

- 安装 [kubectl](https://kubernetes.io/docs/tasks/kubectl/install/)

**Mac OSX**

```shell
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v1.0.1/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

**Linux**

```shell
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v1.0.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

### 启动

缺省Minikube使用VirtualBox驱动来创建Kubernetes本地环境

```
minikube start --registry-mirror=https://registry.docker-cn.com
```

支持不同的Kubernetes版本

```
# 安装Kubernetes v1.12.1
minikube start --registry-mirror=https://registry.docker-cn.com --kubernetes-version v1.12.1
```

打开Kubernetes控制台

```
minikube dashboard
```