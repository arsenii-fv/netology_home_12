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
arsen@aurora:/data/12-ansible/13-kuber-5$ kubectl get pods -A
NAMESPACE        NAME                                        READY   STATUS      RESTARTS         AGE
kube-system      csi-nfs-controller-f9bd9cfc-26ccf           3/3     Running     6 (4d1h ago)     5d13h
kube-system      csi-nfs-node-zq84h                          3/3     Running     6 (4d1h ago)     5d13h
kube-system      hostpath-provisioner-69cd9ff5b8-9dchk       1/1     Running     2 (4d1h ago)     5d10h
kube-system      calico-node-xpsrv                           1/1     Running     3 (4d1h ago)     7d1h
kube-system      calico-kube-controllers-7d9855fcdf-mz2k7    1/1     Running     3 (4d1h ago)     7d1h
metallb-system   speaker-k7dx7                               1/1     Running     149 (4d1h ago)   8d
kube-system      coredns-6f5f9b5d74-pkxdk                    1/1     Running     4 (4d1h ago)     8d
ingress          nginx-ingress-microk8s-controller-xn5vw     1/1     Running     4 (4d1h ago)     7d18h
metallb-system   controller-9556c586f-b7652                  1/1     Running     4 (4d1h ago)     8d
alien            network-multitool                           1/1     Running     0                35h
default          release-name-sky-chart-0-test-connection    0/1     Completed   0                12h
app1             release-name-sky-chart-0-55fbff65b7-txnzz   1/1     Running     0                28s
app1             release-name-sky-chart-0-55fbff65b7-kj445   1/1     Running     0                28s
app1             release-name-sky-chart-0-test-connection    0/1     Completed   0                28s
app1             release-name-sky-chart-1-7cfb5f54c9-9xgwq   1/1     Running     0                18s
app1             release-name-sky-chart-1-7cfb5f54c9-pk4zb   1/1     Running     0                18s
app1             release-name-sky-chart-1-test-connection    0/1     Completed   0                18s
app2             release-name-sky-chart-2-0                  1/1     Running     0                5s
app2             release-name-sky-chart-2-1                  1/1     Running     0                4s
app2             release-name-sky-chart-2-test-connection    0/1     Completed   0                5s
```
```bash
arsen@aurora:/data/12-ansible/13-kuber-5$ kubectl describe pod release-name-sky-chart-2-test-connection -n app2
Name:             release-name-sky-chart-2-test-connection
Namespace:        app2
Priority:         0
Service Account:  default
Node:             atman-v/192.168.1.3
Start Time:       Fri, 07 Apr 2023 11:58:41 +0300
Labels:           app.kubernetes.io/instance=release-name
                  app.kubernetes.io/managed-by=Helm
                  app.kubernetes.io/name=sky-chart-2
                  app.kubernetes.io/version=1.19.5
                  helm.sh/chart=sky-chart-2-0.1.2
Annotations:      cni.projectcalico.org/containerID: e41292cb1b756293d080231d1351dfad68d880b90e4c10986373ea797fa99e8e
                  cni.projectcalico.org/podIP: 
                  cni.projectcalico.org/podIPs: 
                  helm.sh/hook: test
Status:           Succeeded
IP:               10.1.154.35
IPs:
  IP:  10.1.154.35
Containers:
  wget:
    Container ID:  containerd://619299beba5b5db7ac9068d0da44b3a3d5a967c783ce18dedc6301e69bd70528
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:b5d6fe0712636ceb7430189de28819e195e8966372edfc2d9409d79402a0dc16
    Port:          <none>
    Host Port:     <none>
    Command:
      wget
    Args:
      release-name-sky-chart-2:80
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Fri, 07 Apr 2023 11:58:43 +0300
      Finished:     Fri, 07 Apr 2023 11:58:44 +0300
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-d8l4l (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  kube-api-access-d8l4l:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>
```

### Правила приема работы

1. Домашняя работа оформляется в своем Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, `helm`, а также скриншоты результатов
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md

