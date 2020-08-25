# enzo1920_platform
enzo1920 Platform repository


## Kubernetes networks
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
