# Домашнее задание к занятию «Хранение в K8s. Часть 2»

### Цель задания

В тестовой среде Kubernetes нужно создать PV и продемострировать запись и хранение файлов.

------

### Чеклист готовности к домашнему заданию

1. Установленное K8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключенным GitHub-репозиторием.

------

### Дополнительные материалы для выполнения задания

1. [Инструкция по установке NFS в MicroK8S](https://microk8s.io/docs/nfs). 
2. [Описание Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/). 
3. [Описание динамического провижининга](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/). 
4. [Описание Multitool](https://github.com/wbitt/Network-MultiTool).

------

### Задание 1

**Что нужно сделать**

Создать Deployment приложения, использующего локальный PV, созданный вручную.

1. Создать Deployment приложения, состоящего из контейнеров busybox и multitool.
2. Создать PV и PVC для подключения папки на локальной ноде, которая будет использована в поде.
3. Продемонстрировать, что multitool может читать файл, в который busybox пишет каждые пять секунд в общей директории. 
4. Продемонстрировать, что файл сохранился на локальном диске ноды, а также что произойдёт с файлом после удаления пода и deployment. Пояснить, почему.
5. Предоставить манифесты, а также скриншоты или вывод необходимых команд.
```bash
arsen@aurora:/data/12-ansible/13-kuber-2$ kubectl exec busy-multyt-dep-5f49648466-56kmn -c network-multitool -it -- sh
/ # cd /tmp/svf/
/tmp/svf # ls
date.d
/tmp/svf # cat date.d 
21:32:10
21:32:11
21:32:15
21:32:16
21:32:20
21:32:21
21:32:25
21:32:26
21:32:30
21:32:31
21:32:35
21:32:36
21:32:40
21:32:41
21:32:45
21:32:46
21:32:50
21:32:51
21:32:55
21:32:56
21:33:00
21:33:01

arsen@aurora:/data/12-ansible/13-kuber-2$ ssh arsen@192.168.1.3
arsen@192.168.1.3's password: 
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-69-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Apr  2 09:41:30 PM UTC 2023

  System load:  0.38232421875      Processes:               219
  Usage of /:   59.7% of 16.61GB   Users logged in:         1
  Memory usage: 33%                IPv4 address for enp0s3: 192.168.1.3
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro

Expanded Security Maintenance for Applications is not enabled.

28 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


Last login: Thu Mar 30 08:36:47 2023 from 192.168.1.61
arsen@atman-v:~$ cd /tmp/svf/
arsen@atman-v:/tmp/svf$ cat date.d 
21:32:10
21:32:11
21:32:15
21:32:16
21:32:20
21:32:21
21:32:25
21:32:26
21:32:30
21::32:31
21:32:35
21:32:36
21:32:40
21:32:41

После удаления deployment поды находятся в состоянии Terminating, в это время данные в файл продолжают сохраняться.
По истечении 30 секунд после удаления подов, данные в файл прекращают сохраняться.
default          busy-multyt-dep-5f49648466-56kmn           2/2     Terminating   0               12m
default          busy-multyt-dep-5f49648466-gt9dk           2/2     Terminating   0               12m
```
------

### Задание 2

**Что нужно сделать**

Создать Deployment приложения, которое может хранить файлы на NFS с динамическим созданием PV.

1. Включить и настроить NFS-сервер на MicroK8S.
2. Создать Deployment приложения состоящего из multitool, и подключить к нему PV, созданный автоматически на сервере NFS.
3. Продемонстрировать возможность чтения и записи файла изнутри пода. 
4. Предоставить манифесты, а также скриншоты или вывод необходимых команд.
```bash
arsen@aurora:/data/12-ansible/13-kuber-2$ kubectl get pod
NAME                          READY   STATUS    RESTARTS   AGE
multyt-dep-65557758cd-l2rw7   1/1     Running   0          10s
multyt-dep-65557758cd-bcl2m   1/1     Running   0          5s

rsen@aurora:/data/12-ansible/13-kuber-2$ kubectl exec multyt-dep-65557758cd-bcl2m  -c network-multitool -it -- sh
/ # cd /tmp/
/tmp # ls
nfs
/tmp # cd nfs
/tmp/nfs # ls
/tmp/nfs # vi file
/tmp/nfs # ls
file
/tmp/nfs # vi file2
/tmp/nfs # ls
file   file2

arsen@aurora:~$ ssh arsen@192.168.1.3
arsen@atman-v:/$ cd /srv/nfs/
arsen@atman-v:/srv/nfs$ ls
pvc-2ca48af5-7042-4194-b1f8-7be9570e141d
arsen@atman-v:/srv/nfs$ cd pvc-2ca48af5-7042-4194-b1f8-7be9570e141d/
arsen@atman-v:/srv/nfs/pvc-2ca48af5-7042-4194-b1f8-7be9570e141d$ ls -la
total 16
drwxr-xr-x 2 nobody nogroup 4096 Apr  3 07:47 .
drwxrwxrwx 3 nobody nogroup 4096 Apr  3 07:47 ..
-rw-r--r-- 1 nobody nogroup    9 Apr  3 07:46 file
-rw-r--r-- 1 nobody nogroup   13 Apr  3 07:47 file2
```
------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
