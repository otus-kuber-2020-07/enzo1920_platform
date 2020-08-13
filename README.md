# enzo1920_platform
enzo1920 Platform repository


## Nginx
В ходе выполнения дз1 было собран контейнер и загружен на Docker hub.
Была создана  html страничка "kuber intro"
Для в dockerfile был использован файл NGINX Docker Maintainers
в образе создается пользователь и группа nginx с  gid и uid 1001.
и пробрасывается порт. Также меняется конфиг nginx. Создается директория /app 
для хранения странички homework.html

запускается контейнер 
```
docker run -d -p 8000:8000 nginx-test:valve
```

## Kuber
Задание по написанию yaml было сделано по инструкция и официальной документации на kuber

Единственные проблемы -это запуск minikube на хостинге. В качестве драйвера я использовал virtualbox driver
Существуют и другие, но настраиваются по разному kvm2 и podman