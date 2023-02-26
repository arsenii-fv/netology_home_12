# Домашнее задание к занятию "Запуск приложений в K8S"

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Deployment с приложением, состоящим из нескольких контейнеров и масштабировать его.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S)
2. Установленный локальный kubectl
3. Редактор YAML-файлов с подключенным git-репозиторием

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Deployment и примеры манифестов
2. [Описание](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) Init-контейнеров
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool

------

### Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod'а

1. Создать Deployment приложения состоящего из двух контейнеров - nginx и multitool. Решить возникшую ошибку
2. После запуска увеличить кол-во реплик работающего приложения до 2
3. Продемонстрировать кол-во подов до и после масштабирования
```shell
arsen@aurora:/data/12-ansible/12-kuber-3$ kubectl get pods
NAME                                      READY   STATUS        RESTARTS   AGE
nginx-multy-deployment-7ff9d7d489-xlxvp   0/2     Terminating   0          6m47s
nginx-multy-deployment-7ff9d7d489-lwzz2   2/2     Running       0          4s

arsen@aurora:/data/12-ansible/12-kuber-3$ kubectl get pods
NAME                                      READY   STATUS        RESTARTS   AGE
nginx-multy-deployment-7ff9d7d489-xlxvp   0/2     Terminating   0          29m
nginx-multy-deployment-5ff95cf7d4-kvs7p   2/2     Running       0          25s
nginx-multy-deployment-5ff95cf7d4-cckzr   2/2     Running       0          25s
```
4. Создать Service, который обеспечит доступ до реплик приложений из п.1
5. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl` что из пода есть доступ до приложений из п.1
```shell
arsen@aurora:~$ kubectl exec network-multitool-v4wwt -- curl 10.1.154.53:80
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
100   612  100   612    0     0   3079      0 --:--:-- --:--:-- --:--:--  3090

arsen@aurora:~$ kubectl exec network-multitool-v4wwt -- curl 10.1.154.53:1180
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   159  100   159    0     0   1611      0 --:--:-- --:--:-- --:--:--  1606
WBITT Network MultiTool (with NGINX) - nginx-multy-deployment-5ff95cf7d4-kvs7p - 10.1.154.53 - HTTP: 1180 , HTTPS: 11443 . (Formerly praqma/network-multitool)
```
------

### Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

1. Создать Deployment приложения nginx и обеспечить старт контейнера только после того, как будет запущен сервис этого приложения
2. Убедиться, что nginx не стартует. В качестве init-контейнера взять busybox
3. Создать и запустить Service. Убедиться, что nginx запустился
4. Продемонстрировать состояние пода до и после запуска сервиса
```shell
arsen@aurora:/data/12-ansible/12-kuber-3$ kubectl get pods
NAME                                READY   STATUS     RESTARTS   AGE
nginx-deployment-847cf7dbbd-xl8z2   0/1     Init:0/1   0          6s
nginx-deployment-847cf7dbbd-4zn95   0/1     Init:0/1   0          6s
nginx-deployment-847cf7dbbd-hpcp4   0/1     Init:0/1   0          6s
nginx-deployment-847cf7dbbd-ljqj6   0/1     Init:0/1   0          6s

arsen@aurora:/data/12-ansible/12-kuber-3$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-847cf7dbbd-xl8z2   1/1     Running   0          54s
nginx-deployment-847cf7dbbd-4zn95   1/1     Running   0          54s
nginx-deployment-847cf7dbbd-hpcp4   1/1     Running   0          54s
nginx-deployment-847cf7dbbd-ljqj6   1/1     Running   0          54s
```
------

### Правила приема работы

1. Домашняя работа оформляется в своем Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md

------
