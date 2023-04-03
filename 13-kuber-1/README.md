# Домашнее задание к занятию «Хранение в K8s. Часть 1»

### Цель задания

В тестовой среде Kubernetes нужно обеспечить обмен файлами между контейнерам пода и доступ к логам ноды.

------

### Чеклист готовности к домашнему заданию

1. Установленное K8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным GitHub-репозиторием.

------

### Дополнительные материалы для выполнения задания

1. [Инструкция по установке MicroK8S](https://microk8s.io/docs/getting-started).
2. [Описание Volumes](https://kubernetes.io/docs/concepts/storage/volumes/).
3. [Описание Multitool](https://github.com/wbitt/Network-MultiTool).

------

### Задание 1 

**Что нужно сделать**

Создать Deployment приложения, состоящего из двух контейнеров и обменивающихся данными.

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool.
2. Сделать так, чтобы busybox писал каждые пять секунд в некий файл в общей директории.
3. Обеспечить возможность чтения файла контейнером multitool.
4. Продемонстрировать, что multitool может читать файл, который периодоически обновляется.
5. Предоставить манифесты Deployment в решении, а также скриншоты или вывод команды из п. 4.

```bash

arsen@aurora:/data/12-ansible/13-kuber-1$ kubectl exec busy-multyt-dep-65c4554b4f-wfspx -c network-multitool -it -- sh
/ # cd /tmp/multy/
/tmp/multy # ls
date.d
/tmp/multy # cat date.d 
09:03:25
09:03:30
09:03:35
09:03:40
09:03:45
09:03:50
09:03:55
09:04:00
09:04:05
/tmp/multy # cat date.d 
09:03:25
09:03:30
09:03:35
09:03:40
09:03:45
09:03:50
09:03:55
09:04:00
09:04:05
/tmp/multy # cat date.d 
09:03:25
09:03:30
09:03:35
09:03:40
09:03:45
09:03:50
09:03:55
09:04:00
09:04:05
09:04:10
/tmp/multy # cat date.d 
09:03:25
09:03:30
09:03:35
09:03:40
09:03:45
09:03:50
09:03:55
09:04:00
09:04:05
09:04:10
09:04:15
```
------

### Задание 2

**Что нужно сделать**

Создать DaemonSet приложения, которое может прочитать логи ноды.

1. Создать DaemonSet приложения, состоящего из multitool.
2. Обеспечить возможность чтения файла `/var/log/syslog` кластера MicroK8S.
3. Продемонстрировать возможность чтения файла изнутри пода.
4. Предоставить манифесты Deployment, а также скриншоты или вывод команды из п. 2.

arsen@aurora:/data/12-ansible/13-kuber-1$ kubectl get pods
NAME                         READY   STATUS    RESTARTS        AGE
nginx-dep-75fb8b6d8d-qcl8n   1/1     Running   1 (3h30m ago)   19h
nginx-dep-75fb8b6d8d-rqp6z   1/1     Running   1 (3h30m ago)   19h
nginx-dep-75fb8b6d8d-s9zmk   1/1     Running   1 (3h30m ago)   19h
multy-dep-c89dcc49c-4chxx    1/1     Running   1 (3h30m ago)   19h
busy-multyt-ds-q9zfm         1/1     Running   0               51m

arsen@aurora:/data/12-ansible/13-kuber-1$ kubectl exec busy-multyt-ds-q9zfm  -c network-multitool -it -- sh
/ # cd /tmp/var
/tmp/var # ls
syslog_host

/tmp/var # tail -2 syslog_host 
Mar 31 10:39:12 atman-v microk8s.daemon-kubelite[994034]: I0331 10:39:12.851935  994034 scope.go:115] "RemoveContainer" containerID="56418cf3c99fb02e67cc42cabd6671cd694a89b02a1255ac6bb1675828c94b9b"
Mar 31 10:39:12 atman-v microk8s.daemon-kubelite[994034]: E0331 10:39:12.852193  994034 pod_workers.go:965] "Error syncing pod, skipping" err="failed to \"StartContainer\" for \"speaker\" with CrashLoopBackOff: \"back-off 5m0s restarting failed container=speaker pod=speaker-k7dx7_metallb-system(f092d0fe-7696-426c-ab29-48edba713fd6)\"" pod="metallb-system/speaker-k7dx7" podUID=f092d0fe-7696-426c-ab29-48edba713fd6


------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------
