# Домашнее задание к занятию "Управление доступом"

### Цель задания

В тестовой среде Kubernetes необходимо предоставить ограниченный доступ пользователю.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S)
2. Установленный локальный kubectl
3. Редактор YAML-файлов с подключенным github-репозиторием

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) RBAC
2. [Пользователи и авторизация RBAC в Kubernetes](https://habr.com/ru/company/flant/blog/470503/)
3. [RBAC with Kubernetes in Minikube](https://medium.com/@HoussemDellai/rbac-with-kubernetes-in-minikube-4deed658ea7b)

------

### Задание 1. Создать конфигурацию для подключения пользователя

1. Создать и подписать SSL-сертификат для подключения к кластеру.
2. Настроить конфигурационный файл kubectl для подключения
3. Создать Роли и все необходимые настройки для пользователя
4. Предусмотреть права пользователя. Пользователь может просматривать логи подов и их конфигурацию (`kubectl logs pod <pod_id>`, `kubectl describe pod <pod_id>`)
5. Предоставить манифесты, а также скриншоты и/или вывод необходимых команд.
```bash
arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl config use-context guest-con
Switched to context "guest-con".

arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl logs network-multitool -n alien
The directory /usr/share/nginx/html is not mounted.
Therefore, over-writing the default index.html file with some useful information:
WBITT Network MultiTool (with NGINX) - network-multitool - 10.1.154.40 - HTTP: 1180 , HTTPS: 11443 . (Formerly praqma/network-multitool)
Replacing default HTTP port (80) with the value specified by the user - (HTTP_PORT: 1180).
Replacing default HTTPS port (443) with the value specified by the user - (HTTPS_PORT: 11443).

arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl logs network-multitool -n alien
The directory /usr/share/nginx/html is not mounted.
Therefore, over-writing the default index.html file with some useful information:
WBITT Network MultiTool (with NGINX) - network-multitool - 10.1.154.40 - HTTP: 1180 , HTTPS: 11443 . (Formerly praqma/network-multitool)
Replacing default HTTP port (80) with the value specified by the user - (HTTP_PORT: 1180).
Replacing default HTTPS port (443) with the value specified by the user - (HTTPS_PORT: 11443).
arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl describe network-multitool -n alien
error: the server doesn't have a resource type "network-multitool"
arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl describe pod network-multitool -n alien
Name:             network-multitool
Namespace:        alien
Priority:         0
Service Account:  default
Node:             atman-v/192.168.1.3
Start Time:       Thu, 06 Apr 2023 00:47:02 +0300
Labels:           app=network-multitool
Annotations:      cni.projectcalico.org/containerID: e5fae07c27aa014ea392e09cd64e0a30618479db7f4a8c40253feb3b246b81b6
                  cni.projectcalico.org/podIP: 10.1.154.40/32
                  cni.projectcalico.org/podIPs: 10.1.154.40/32
Status:           Running
IP:               10.1.154.40
IPs:
  IP:  10.1.154.40
Containers:
  network-multitool:
    Container ID:   containerd://c67256165b0844d101771e01486b4e154d6393ea3e63456cb50251e070f0c56f
    Image:          wbitt/network-multitool
    Image ID:       docker.io/wbitt/network-multitool@sha256:82a5ea955024390d6b438ce22ccc75c98b481bf00e57c13e9a9cc1458eb92652
    Ports:          1180/TCP, 11443/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Running
      Started:      Thu, 06 Apr 2023 00:47:06 +0300
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     10m
      memory:  20Mi
    Requests:
      cpu:     1m
      memory:  20Mi
    Environment:
      HTTP_PORT:   1180
      HTTPS_PORT:  11443
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-49vp7 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-49vp7:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>

arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl get pods -A
Error from server (Forbidden): pods is forbidden: User "guest" cannot list resource "pods" in API group "" at the cluster scope
arsen@aurora:/data/12-ansible/13-kuber-4$ kubectl get pods -n alien
NAME                READY   STATUS    RESTARTS   AGE
network-multitool   1/1     Running   0          9h
```
------

### Правила приема работы

1. Домашняя работа оформляется в  своем Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, а также скриншоты результатов
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md

------
