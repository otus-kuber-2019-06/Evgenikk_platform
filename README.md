# Evgenikk_platform
Evgenikk Platform repository
### kubernetes-intro

minikube pods initcontainer
Полезные команды:

```
kubectl logs pod-name -c container-name # Inspect the first init container
kubectl apply -f web-pod.yaml && kubectl get pods -w
```
#### Как запускаются элементы k8s в minikube
```
 pixel@Evgeniys-MacBook-Pro ~/git_workspace/otus/Evgenikk_platform kubectl get ds -n kube-system                               
NAME         DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
kube-proxy   1         1         1         1            1           beta.kubernetes.io/os=linux   45m
```

```
 pixel@Evgeniys-MacBook-Pro  ~/git_workspace/otus/Evgenikk_platform  kubectl get deployment -n kube-system       
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
coredns   2         2         2            2           47m
```

Все остальные сущности (АPI, планировщик и прочее) запускаются через kubelet в подах (restart policy = always ). Kubelet описан в сервисе для systemd.
```
   CGroup: /system.slice/kubelet.service
           └─3225 /usr/bin/kubelet --authorization-mode=Webhook --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --cgroup-driver=cgroupfs --client-ca-file=/var/lib/minikube/certs/ca.crt --cluster-dns=10.96.0.10 --cluster-domain=cluster.local --container-runtime=docker --fail-swap-on=false --hostname-override=minikube --kubeconfig=/etc/kubernetes/kubelet.conf --pod-manifest-path=/etc/kubernetes/manifests
```
Здесь, стоит обратить внимание на --pod-manifest-path=/etc/kubernetes/manifests - путь к манифестам api сервера и тд


