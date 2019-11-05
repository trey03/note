# k8s others

```text
kubeadm init \
  --apiserver-advertise-address=192.168.99.103 \
  --image-repository reg.i3hh.com:8091/k8s \
  --kubernetes-version v1.16.2 \
  --service-cidr=10.1.0.0/16 \
  --pod-network-cidr=10.244.0.0/16
  
kubectl taint nodes --all node-role.kubernetes.io/master-
kubectl apply -f 
https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml

kubectl apply -f 
https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta5/aio/deploy/recommended.yaml

kubectl proxy --address='192.168.99.103' disable-filter=true --accept-hosts="^192.168.99.103$"
kubectl proxy --address='192.168.99.103' disable-filter=true --accept-hosts="^192.168.99.103$"
kubectl proxy --address 0.0.0.0 --accept-hosts '.*'
```

[http://192.168.99.103:31880/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/\#/login](http://192.168.99.103:31880/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login)

