# Домашнее задание к занятию "Сетевое взаимодействие в K8S. Часть 1"

### Цель задания

В тестовой среде Kubernetes необходимо обеспечить доступ к приложению, установленному в предыдущем ДЗ и состоящему из двух контейнеров, по разным портам в разные контейнеры как внутри кластера, так и снаружи.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S)
2. Установленный локальный kubectl
3. Редактор YAML-файлов с подключенным git-репозиторием

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Deployment и примеры манифестов
2. [Описание](https://kubernetes.io/docs/concepts/services-networking/service/) Описание Service
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool

------

### Задание 1. Создать Deployment и обеспечить доступ к контейнерам приложения по разным портам из другого Pod'а внутри кластера

1. Создать Deployment приложения, состоящего из двух контейнеров - nginx и multitool с кол-вом реплик 3шт.
2. Создать Service, который обеспечит доступ внутри кластера до контейнеров приложения из п.1 по порту 9001 - nginx 80, по 9002 - multitool 8080.
3. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложения из п.1 по разным портам в разные контейнеры
```shell
arsen@aurora:/data/12-ansible/12-kuber-4$ kubectl exec network-multitool-nz4f4  -- curl 10.152.183.132:9001
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   612  100   612    0     0   6183      0 --:--:-- --:--:-- --:--:--  6181
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

arsen@aurora:/data/12-ansible/12-kuber-4$ kubectl exec network-multitool-nz4f4  -- curl 10.152.183.132:9002
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   158  100   158    0     0    530      0 --:--:-- --:--:-- --:--:--   530
WBITT Network MultiTool (with NGINX) - nginx-multy-deployment-7ccbc546d8-2fbv8 - 10.1.154.6 - HTTP: 8080 , HTTPS: 11443 . (Formerly praqma/network-multitool)
```
4. Продемонстрировать доступ с помощью `curl` по доменному имени сервиса.
```shell
arsen@aurora:/data/12-ansible/12-kuber-4$ kubectl exec network-multitool-nz4f4  -- curl netology-svc1:9001
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
100   612  100   612    0     0   3077      0 --:--:-- --:--:-- --:--:--  597k

arsen@aurora:/data/12-ansible/12-kuber-4$ kubectl exec network-multitool-nz4f4  -- curl netology-svc1:9002
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0WBITT Network MultiTool (with NGINX) - nginx-multy-deployment-7ccbc546d8-2fbv8 - 10.1.154.6 - HTTP: 8080 , HTTPS: 11443 . (Formerly praqma/network-multitool)
100   158  100   158    0     0    533      0 --:--:-- --:--:-- --:--:--  1564
5. Предоставить манифесты Deployment'а и Service в решении, а также скриншоты или вывод команды п.4
```
------

### Задание 2. Создать Service и обеспечить доступ к приложениям снаружи кластера

1. Создать отдельный Service приложения из Задания 1 с возможностью доступа снаружи кластера к nginx используя тип NodePort.
2. Продемонстрировать доступ с помощью браузера или `curl` с локального компьютера.

```shell
arsen@aurora:/data/12-ansible/12-kuber-4$ curl 192.168.1.2:31001
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

arsen@aurora:/data/12-ansible/12-kuber-4$ curl 192.168.1.2:31002
WBITT Network MultiTool (with NGINX) - nginx-multy-deployment-7ccbc546d8-bc825 - 10.1.154.22 - HTTP: 8080 , HTTPS: 11443 . (Formerly praqma/network-multitool)
```
3. Предоставить манифест и Service в решении, а также скриншоты или вывод команды п.2.

------

### Правила приема работы

1. Домашняя работа оформляется в своем Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

