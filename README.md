# enzo1920_platform
enzo1920 Platform repository


## Kubernetes volumes
� ���� �� ���� ��������� ������� �� ���������:
���������� �������� Pod
�������� ������� Deployment
���������� �������� � ������� ( ClusterIP )
��������� ������ ������������ IPVS

��������� MetalLB � Layer2-������ // ��� ������� ��������� ��� bad practice
���������� ������� LoadBalancer
��������� Ingress-����������� � ������ ingress-nginx
�������� ������ Ingress

����� ���� ��������� ������� �� *:
��������� �������� � coredns �����
����� ���������� �����. 
����� ���������� ����� ����� ����������:

```
kubectl -n kube-system get deployment coredns -o yaml
```

�������, ��� ����� 53 ����� �������� ���� 9153
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
task with[*]: https://ealebed.github.io/posts/2018/знакомство-с-kubernetes-часть-14-секреты-secrets/
