# enzo1920_platform
enzo1920 Platform repository


## Nginx
� ���� ���������� ��1 ���� ������ ��������� � �������� �� Docker hub.
���� �������  html ��������� "kuber intro"
��� � dockerfile ��� ����������� ���� NGINX Docker Maintainers
� ������ ��������� ������������ � ������ nginx �  gid � uid 1001.
� �������������� ����. ����� �������� ������ nginx. ��������� ���������� /app 
��� �������� ��������� homework.html

����������� ��������� 
```
docker run -d -p 8000:8000 nginx-test:valve
```

## Kuber
������� �� ��������� yaml ���� ������� �� ���������� � ����������� ������������ �� kuber

������������ �������� -��� ������ minikube �� ��������. � �������� �������� � ����������� virtualbox driver
���������� � ������, �� ������������� �� ������� kvm2 � podman