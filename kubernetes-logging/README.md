#### Выполненное logging
Написано ридми по хистори, некоторые вещи в методичке
0)кластер настроен через вэб интерфейс
1)gcloud container clusters get-credentials cluster-z31 --zone us-central1-c
2)Далее проведена настройка нод(taint) чтобы ничего лишнего не ставилось на указанные ноды по умолчанию
```
kubectl taint nodes gke-cluster-z31-infra-pool-4462b8b6-0r8p node-role=infra:NoSchedule
kubectl taint nodes gke-cluster-z31-infra-pool-4462b8b6-6g00 node-role=infra:NoSchedule
kubectl taint nodes gke-cluster-z31-infra-pool-4462b8b6-zwvd  node-role=infra:NoSchedule
kubectl get node
```
3)Установка HipsterShop через helm по методичке
```
kubectl create ns microservices-demo
kubectl apply -f https://raw.githubusercontent.com/express42/otus-platform-snippets/master/Module-02/Logging/microservices-demo-without-resources.yaml -n microservices-demo
```
4)Далее устанавливаем все компоненты для логгирования
```
kubectl get pods -n microservices-demo -o wide
helm repo add elastic https://helm.elastic.co
kubectl create ns observability
helm upgrade --install elasticsearch elastic/elasticsearch --namespace observability
helm upgrade --install kibana elastic/kibana --namespace observability
helm upgrade --install fluent-bit stable/fluent-bit --namespace observability
kubectl get pods --namespace=observability
kubectl get pods --namespace=observability -o wide
```

5)Устанавливаем ingress
```
helm upgrade --install nginx-ingress nginx-stable/nginx-ingress --namespace=nginx-ingress --version=0.6.1 -f nginx-ingress.values.yaml
```

6)Смотрим логи fluent , что там не так
```
kubectl logs fluent-bit-4bsmz -n logging --tail 2
```

Далее по методичке

7)установка prometheus и оператора
```
helm upgrade --install prometheus-operator prometheus-community/kube-prometheus-stack  --namespace=observability -f prometheus-operator-values.yaml
helm upgrade --install elasticsearch-exporter stable/elasticsearch-exporter --set es.uri=http://elasticsearch-master:9200 --set serviceMonitor.enabled=tr ue --namespace=observability
```

8) выводим одну ноду "из строя" и смотрим :)
```
kubectl get node
kubectl drain gke-cluster-z1-infra-pool-95f25ba7-4jqt --ignore-daemonsets
kubectl get node
kubectl uncordon gke-cluster-z1-infra-pool-95f25ba7-4jqt
 ```
9)Теперь получам логи ингресса, поправив fluent-bit.values.yaml(спасибо Kirill Kotov за подсказку)
```
tolerations:
  - key: node-role
    operator: Equal
    value: infra
    effect: NoSchedule
```

```
helm upgrade --install fluent-bit stable/fluent-bit --namespace observability -f fluent-bit.values.yaml
```
10)Далее ставим Loki
```
nano loki-values.yaml
helm repo add loki https://grafana.github.io/loki/charts
helm repo update
helm upgrade --install loki --namespace=observability loki/loki -f loki-values.yaml
```

11) Правим файл значений оператора
```
nano prometheus-operator-values.yaml
helm upgrade --install prometheus-operator prometheus-community/kube-prometheus-stack  --namespace=observability -f prometheus-operator-values.yaml
```
