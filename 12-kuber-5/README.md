# Домашнее задание к занятию "Сетевое взаимодействие в K8S. Часть 2"

### Цель задания

В тестовой среде Kubernetes необходимо обеспечить доступ к двум приложениям снаружи кластера по разным путям.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным git-репозиторием.

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция](https://microk8s.io/docs/getting-started) по установке MicroK8S
2. [Описание](https://kubernetes.io/docs/concepts/services-networking/service/) Service
3. [Описание](https://kubernetes.io/docs/concepts/services-networking/ingress/) Ingress
4. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool

------

### Задание 1. Создать Deployment приложений backend и frontend

1. Создать Deployment приложения _frontend_ из образа nginx с кол-вом реплик 3 шт.
2. Создать Deployment приложения _backend_ из образа multitool. 
3. Добавить Service'ы, которые обеспечат доступ к обоим приложениям внутри кластера. 
4. Продемонстрировать, что приложения видят друг друга с помощью Service.
5. Предоставить манифесты Deployment'а и Service в решении, а также скриншоты или вывод команды п.4.

```bash
arsen@aurora:/data/12-ansible/12-kuber-5$ kubectl get pods -o wide
NAME                         READY   STATUS    RESTARTS       AGE     IP            NODE      NOMINATED NODE   READINESS GATES
nginx-dep-75fb8b6d8d-f5vtf   1/1     Running   1 (136m ago)   7d20h   10.1.154.31   atman-v   <none>           <none>
nginx-dep-75fb8b6d8d-5dnjt   1/1     Running   1 (136m ago)   7d20h   10.1.154.23   atman-v   <none>           <none>
nginx-dep-75fb8b6d8d-vkltm   1/1     Running   1 (136m ago)   7d20h   10.1.154.27   atman-v   <none>           <none>
multy-dep-798dbd8b49-nf9n9   1/1     Running   1 (136m ago)   2d22h   10.1.154.35   atman-v   <none>           <none>
network-multitool-ds9gf      1/1     Running   0              31m     10.1.154.34   atman-v   <none>           <none>

arsen@aurora:/data/12-ansible/12-kuber-5$ kubectl exec multy-dep-798dbd8b49-nf9n9 -- curl nginx-svc:9001
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
100   612  100   612    0     0   3069      0 --:--:-- --:--:-- --:--:--  6181

arsen@aurora:/data/12-ansible/12-kuber-5$ kubectl exec network-multitool-ds9gf -- curl multy-svc:9002
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0WBITT Network MultiTool (with NGINX) - multy-dep-798dbd8b49-nf9n9 - 10.1.154.35 - HTTP: 8080 , HTTPS: 11443 . (Formerly praqma/network-multitool)
100   146  100   146    0     0    735      0 --:--:-- --:--:-- --:--:--  142k
```
------

### Задание 2. Создать Ingress и обеспечить доступ к приложениям снаружи кластера

1. Включить Ingress-controller в microk8s
2. Создать Ingress, обеспечивающий доступ снаружи по IP-адресу кластера microk8s, так чтобы при запросе только по адресу открывался _frontend_ а при добавлении /api - _backend_
3. Продемонстрировать доступ с помощью браузера или `curl` с локального компьютера

```bash
arsen@aurora:/data/12-ansible/12-kuber-5$ kubectl describe ing
Name:             nginx-ingress-microk8s
Labels:           app=nginx-ingress-microk8s
Namespace:        default
Address:          192.168.1.3
Ingress Class:    nginx
Default backend:  <default>
Rules:
  Host        Path  Backends
  ----        ----  --------
  atman-v     
              /      nginx-svc:9001 (10.1.154.32:80,10.1.154.33:80,10.1.154.34:80)
              /api   multy-svc:9002 (10.1.154.31:1180)
Annotations:  nginx.ingress.kubernetes.io/rewrite-target: /$1
Events:       <none>

arsen@aurora:/data/12-ansible/12-kuber-5$ curl -L atman-v/
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

arsen@aurora:/data/12-ansible/12-kuber-5$ curl -L atman-v/api
WBITT Network MultiTool (with NGINX) - multy-dep-c89dcc49c-4chxx - 10.1.154.31 - HTTP: 1180 , HTTPS: 11443 . (Formerly praqma/network-multitool)
```
4. Предоставить манифесты, а также скриншоты или вывод команды п.2

------

### Правила приема работы

1. Домашняя работа оформляется в своем Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md

------
```
 kubectl describe ds nginx-ingress-microk8s-controller -n ingress
 kubectl edit daemonset nginx-ingress-microk8s-controller -n ingress
```
