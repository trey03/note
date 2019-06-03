# istio

* 第一次安装

获取安装包

```text
curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.7 sh -
```

将 bin/istioctl 加入到$PATH 下

执行将istio安装到kubernates中

```text
kubectl apply -f install/kubernetes/istio-demo.yaml
```

观察部署情况

```text
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```

基于minikube，打开网络

```text
minikube tunnel
```

* 更新现有

下载最新版本的后解压进入到istio目前 更新$PAHT上istioclt

更新istio部署

```text
> for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done

> helm template install/kubernetes/helm/istio --name istio \\n--namespace istio-system > ./istio.yaml

> kubectl apply -f istio.yaml
```

* 部署bookinfo且例

```text
kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml)
```

移除bookinfo用例

```text
kubectl delete -f samples/bookinfo/platform/kube/bookinfo.yaml
```

* 安装 helm

  ```text
  brew install kubernetes-helm
  ```

