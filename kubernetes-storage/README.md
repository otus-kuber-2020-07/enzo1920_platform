
Задание выполнено с помощью кластера миникуба
0)создаем кластер миникуба.

1)[Включаем аддон по мануалу](https://minikube.sigs.k8s.io/docs/tutorials/volume_snapshots_and_csi/)

2)Добавляем ноду worker, получается так:

```
ubuntu@srv01:~$ k get nodes
NAME           STATUS   ROLES    AGE     VERSION
minikube       Ready    master   4m45s   v1.19.2
minikube-m02   Ready    <none>   49s     v1.19.2
```

3)Далее по мануалу из первой ссылки яно, что можно как аддон подключить драйвер, но установим его
Ищем в гугле, заходим на гит репо, читаем инструкцию и выполняем:

```
k apply -f https://raw.githubusercontent.com/kubernetes-csi/external-snapshotter/v2.0.1/config/crd/snapshot.storage.k8s.io_volumesnapshotclasses.yaml
k apply -f https://raw.githubusercontent.com/kubernetes-csi/external-snapshotter/v2.0.1/config/crd/snapshot.storage.k8s.io_volumesnapshotcontents.yaml
k apply -f https://raw.githubusercontent.com/kubernetes-csi/external-snapshotter/v2.0.1/config/crd/snapshot.storage.k8s.io_volumesnapshots.yaml

# Create snapshot controller
k apply -f https://raw.githubusercontent.com/kubernetes-csi/external-snapshotter/v2.0.1/deploy/kubernetes/snapshot-controller/rbac-snapshot-controller.yaml
k apply -f https://raw.githubusercontent.com/kubernetes-csi/external-snapshotter/v2.0.1/deploy/kubernetes/snapshot-controller/setup-snapshot-controller.yaml
```
4)Смотрим, по мануалу , что должен подняться контроллер. Так, окей, стартовал.
```
ubuntu@srv01:~$ k get po
NAME                    READY   STATUS    RESTARTS   AGE
snapshot-controller-0   1/1     Running   0          3m59s

```

5) Проверяем поды, тоже все гут

```
ubuntu@srv01:/home/csi-driver-host-path$ k get pods
NAME                         READY   STATUS    RESTARTS   AGE
csi-hostpath-attacher-0      1/1     Running   0          82s
csi-hostpath-provisioner-0   1/1     Running   0          80s
csi-hostpath-resizer-0       1/1     Running   0          80s
csi-hostpath-snapshotter-0   1/1     Running   0          79s
csi-hostpath-socat-0         1/1     Running   0          78s
csi-hostpathplugin-0         3/3     Running   0          81s
snapshot-controller-0        1/1     Running   0          12m
```

6)берем примеры из деректории примеров(csi) немного правим и ставим по методичке
```
ubuntu@srv01:/home/kube_learn/enzo1920_platform/kubernetes-storage/hw$ k apply -f storageclass.yaml                    storageclass.storage.k8s.io/storage-enzo1920 created
ubuntu@srv01:/home/kube_learn/enzo1920_platform/kubernetes-storage/hw$ k apply -f storage-p
storage-pod.yaml  storage-pvc.yaml
ubuntu@srv01:/home/kube_learn/enzo1920_platform/kubernetes-storage/hw$ k apply -f storage-pvc.yaml
persistentvolumeclaim/storage-pvc created
ubuntu@srv01:/home/kube_learn/enzo1920_platform/kubernetes-storage/hw$ k get pvc
NAME          STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE
storage-pvc   Bound    pvc-538faecd-6bcb-4037-9b0a-e321f8eb8031   1Gi        RWO            storage-enzo1920   14s

```

7)Подконнектимся  к поду и создадим файл(тут ошибся, создал два файла один data.txt, второй date.txt записал в один)

```
hw$ k exec -it storage-pod sh
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
/ # cd /data
/data # touch date.txt
/data # ls
date.txt
/data # pwd
/data
/data # date > data.txt
/data # cat data.txt
Thu Dec  3 22:33:55 UTC 2020
/data # exit
```

8) Теперь посмотрим , записалось ли
```
k exec -it csi-hostpathplugin-0  -c hostpath /bin/sh
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
hostpath /bin/sh
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
/ # cd csi
csi-data-dir/  csi/
/ # cd csi-data-dir/
/csi-data-dir # ls -l
total 4
drwxr-xr-x    2 root     root          4096 Dec  3 22:33 f46a2c4e-35b6-11eb-b7d8-0242ac110003
/csi-data-dir # cd f46a2c4e-35b6-11eb-b7d8-0242ac110003/
/csi-data-dir/f46a2c4e-35b6-11eb-b7d8-0242ac110003 # ls -l
total 4
-rw-r--r--    1 root     root            29 Dec  3 22:33 data.txt
-rw-r--r--    1 root     root             0 Dec  3 22:33 date.txt
/csi-data-dir/f46a2c4e-35b6-11eb-b7d8-0242ac110003 # cat date.txt
/csi-data-dir/f46a2c4e-35b6-11eb-b7d8-0242ac110003 # cat data.txt
Thu Dec  3 22:33:55 UTC 2020
/csi-data-dir/f46a2c4e-35b6-11eb-b7d8-0242ac110003 #

```
Видим два файла, в одном время :)))


