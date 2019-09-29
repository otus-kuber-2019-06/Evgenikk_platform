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
kubectl get ds -n kube-system                               
NAME         DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
kube-proxy   1         1         1         1            1           beta.kubernetes.io/os=linux   45m
```

```
kubectl get deployment -n kube-system                        
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
coredns   2         2         2            2           47m
```

Все остальные сущности (АPI, планировщик и прочее) запускаются через kubelet в подах (restart policy = always ). Kubelet описан в сервисе для systemd.
```
   CGroup: /system.slice/kubelet.service
           └─3225 /usr/bin/kubelet --authorization-mode=Webhook --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --cgroup-driver=cgroupfs --client-ca-file=/var/lib/minikube/certs/ca.crt --cluster-dns=10.96.0.10 --cluster-domain=cluster.local --container-runtime=docker --fail-swap-on=false --hostname-override=minikube --kubeconfig=/etc/kubernetes/kubelet.conf --pod-manifest-path=/etc/kubernetes/manifests
```
Здесь, стоит обратить внимание на --pod-manifest-path=/etc/kubernetes/manifests - путь к манифестам api сервера и тд


### Helm шпаргалка:

Посмотреть релизы, используя tiller в определеннои namespace:
```
helm list --tiller-namespace cert-manager 
helm tiller run helm list 
```

По умолчанию плагин helm tiller использует secret для хранения информации о релизах. Чтобы tiller внутри кластера видел релизы, можно заставить использовать helm tiller plugin  configmaps. 
> Warning тогда `helm tiller run helm list` не будет показывать релизы, хранимые в секретах

```
export HELM_TILLER_STORAGE=configmap
export HELM_TILLER_STORAGE=secret
```

Устанавливаем релиз при помощи helm tiller плагина:
```
helm tiller run \
helm upgrade --install chartmuseum stable/chartmuseum --wait \
--namespace=chartmuseum \
--version=2.3.2 \
-f kubernetes-templating/chartmuseum/values.yaml
```

Удаляем:
```
helm tiller run helm delete --purge chartmuseum   
```

helm3 upgrade --install harbor harbor/harbor --wait \
--namespace=harbor \
--version=1.1.2 \
-f kubernetes-templating/harbor/values.yaml
```

Обратите внимание, как helm3 хранит информацию о release:
```
kubectl get secrets -n harbor -l owner=helm
```

Установка + смена значений в values.yaml:
```
helm upgrade --install socks-shop kubernetes-templating/socks-shop --namespace socksshop --set frontend.service.NodePort=31234
```

Секреты:
```
brew install sops
brew install gnupg2
brew install gnu-getopt
helm plugin install https://github.com/futuresimple/helm-secrets --version 2.0.2
```