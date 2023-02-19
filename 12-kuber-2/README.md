# Домашнее задание к занятию "Базовые объекты K8S"

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Pod с приложением и подключиться к нему со своего локального компьютера. 

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S)
2. Установленный локальный kubectl
3. Редактор YAML-файлов с подключенным git-репозиторием

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. Описание [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) и примеры манифестов
2. Описание [Service](https://kubernetes.io/docs/concepts/services-networking/service/)

------

### Задание 1. Создать Pod с именем "hello-world"

1. Создать манифест (yaml-конфигурацию) Pod
```yml
apiVersion: v1
kind: Pod
metadata:
  labels: 
    app: e2e
  name: hello-world
#  namespace: default
spec:
  containers:
  - name: e2e
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
    #   name: web
    #   protocol: TCP
    # imagePullPolicy: IfNotPresent
```

2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2
3. Подключиться локально к Pod с помощью `kubectl port-forward` и вывести значение (curl или в браузере)

```shell
arsen@aurora:/data/12-ansible/12-kuber-2$ kubectl port-forward pod/hello-world 8085:8080
arsen@aurora:~$ curl -L 127.0.0.1:8085
Hostname: hello-world
Pod Information:
	-no pod information available-
Server values:
	server_version=nginx: 1.12.2 - lua: 10010
Request Information:
	client_address=127.0.0.1
	method=GET
	real path=/
	query=
	request_version=1.1
	request_scheme=http
	request_uri=http://127.0.0.1:8080/
Request Headers:
	accept=*/*  
	host=127.0.0.1:8085  
	user-agent=curl/7.74.0  
Request Body:
	-no body in request-
```
------
### Задание 2. Создать Service и подключить его к Pod
1. Создать Pod с именем "netology-web"
```yml
apiVersion: v1
kind: Pod
metadata:
  labels: 
    app: e2e
  name: netology-web
#  namespace: default
spec:
  containers:
  - name: e2e
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```
2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2

3. Создать Service с именем "netology-svc" и подключить к "netology-web"
```yml
apiVersion: v1
kind: Service
metadata:
  name: netology-svc
#  namespace: default
spec:
  selector: 
    app: e2e
  ports:
  - name: service-port
    #targetPort: echoserver-web
    port: 8080
    #protocol: TCP
    # imagePullPolicy: IfNotPresent
```
4. Подключиться локально к Service с помощью `kubectl port-forward` и вывести значение (curl или в браузере)

```shell
arsen@aurora:/data/12-ansible/12-kuber-2$ kubectl port-forward svc/netology-svc 8086:8080

arsen@aurora:~$ curl -L localhost:8086
Hostname: netology-web
Pod Information:
	-no pod information available-
Server values:
	server_version=nginx: 1.12.2 - lua: 10010
Request Information:
	client_address=127.0.0.1
	method=GET
	real path=/
	query=
	request_version=1.1
	request_scheme=http
	request_uri=http://localhost:8080/
Request Headers:
	accept=*/*  
	host=localhost:8086  
	user-agent=curl/7.74.0  
Request Body:
	-no body in request-
```
------
### Правила приема работы

1. Домашняя работа оформляется в своем  Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get pods`, а также скриншот результата подключения
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md

------

### Критерии оценки
Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
