# Домашнее задание к занятию "Helm"

### Цель задания

В тестовой среде Kubernetes необходимо установить и обновить приложения с помощью Helm.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S)
2. Установленный локальный kubectl
3. Установленный локальный helm
4. Редактор YAML-файлов с подключенным github-репозиторием

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция](https://helm.sh/docs/intro/install/) по установке Helm. [Helm completion](https://helm.sh/docs/helm/helm_completion/)

------

### Задание 1. Подготовить helm чарт для приложения

1. Необходимо упаковать приложение в чарт для деплоя в разные окружения.
2. Каждый компонент приложения деплоится отдельным deployment’ом/statefulset’ом/
3. В переменных чарта измените образ приложения для изменения версии.

------
### Задание 2. Запустить 2 версии в разных неймспейсах

1. Подготовив чарт, необходимо его проверить. Запуститe несколько копий приложения.
2. Одну версию в namespace=app1, вторую версию в том же неймспейсе;третью версию в namespace=app2.
3. Продемонстрируйте результат/

```bash
atman@aurora:~/Desktop/data/12-ansible/13-kuber-5$ kubectl get pods -A
NAMESPACE        NAME                                        READY   STATUS    RESTARTS        AGE
kube-system      csi-nfs-controller-f9bd9cfc-26ccf           3/3     Running   6 (11d ago)     13d
kube-system      csi-nfs-node-zq84h                          3/3     Running   6 (11d ago)     13d
kube-system      hostpath-provisioner-69cd9ff5b8-9dchk       1/1     Running   2 (11d ago)     12d
kube-system      calico-node-xpsrv                           1/1     Running   3 (11d ago)     14d
kube-system      calico-kube-controllers-7d9855fcdf-mz2k7    1/1     Running   3 (11d ago)     14d
metallb-system   speaker-k7dx7                               1/1     Running   149 (11d ago)   15d
kube-system      coredns-6f5f9b5d74-pkxdk                    1/1     Running   4 (11d ago)     15d
ingress          nginx-ingress-microk8s-controller-xn5vw     1/1     Running   4 (11d ago)     15d
metallb-system   controller-9556c586f-b7652                  1/1     Running   4 (11d ago)     15d
alien            network-multitool                           1/1     Running   0               8d
app2             release-name-sky-chart-2-0                  1/1     Running   0               7d12h
app2             release-name-sky-chart-2-1                  1/1     Running   0               7d12h
app2             release-name-sky-chart-2-2                  1/1     Running   0               12m
app1             release-name-sky-chart-1-7cfb5f54c9-vjfnh   1/1     Running   0               2m32s
app1             release-name-sky-chart-1-7cfb5f54c9-hvrmh   1/1     Running   0               2m32s
app1             release-name-sky-chart-1-7cfb5f54c9-txlwr   1/1     Running   0               2m32s
app1             release-name-sky-chart-0-55fbff65b7-5q8x9   1/1     Running   0               66s
app1             release-name-sky-chart-0-55fbff65b7-rl68n   1/1     Running   0               66s
app1             release-name-sky-chart-0-55fbff65b7-t56c9   1/1     Running   0               66s
```

### Правила приема работы

1. Домашняя работа оформляется в своем Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, `helm`, а также скриншоты результатов
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md
