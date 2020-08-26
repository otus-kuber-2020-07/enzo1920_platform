# enzo1920_platform
enzo1920 Platform repository


## Kubernetes volumes
В ходе дз были выполнены задания по методичке:
Добавление проверок Pod
Создание объекта Deployment
Добавление сервисов в кластер ( ClusterIP )
Включение режима балансировки IPVS

Установка MetalLB в Layer2-режиме // тут хорошее замечание про bad practice
Добавление сервиса LoadBalancer
Установка Ingress-контроллера и прокси ingress-nginx
Создание правил Ingress

Также было выполнено задание со *:
настройка запросов к coredns извне
Очень интересный пункт. 
Решил посмотреть какие порты проброшены:

```
kubectl -n kube-system get deployment coredns -o yaml
```

выяснил, что кроме 53 порта работает порт 9153
```
 ports:
        - containerPort: 53
          name: dns
          protocol: UDP
        - containerPort: 53
          name: dns-tcp
          protocol: TCP
        - containerPort: 9153
          name: metrics
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          httpGet:
```
task with[*]: https://ealebed.github.io/posts/2018/Р·РЅР°РєРѕРјСЃС‚РІРѕ-СЃ-kubernetes-С‡Р°СЃС‚СЊ-14-СЃРµРєСЂРµС‚С‹-secrets/
