# Установка кластера Kubernetes (k0s)
#### Подготовка 
##### Загрузить командную утилиту [k0sctl](https://github.com/k0sproject/k0sctl)
```sh
# curl -Lv https://github.com/k0sproject/k0sctl/releases/download/v0.15.0/k0sctl-linux-x64 -o /usr/bin/k0sctl
```
##### Выставить разрешения на запуск
```sh
# chmod +x /usr/bin/k0sctl
```
##### Сгенерировать ключевую пару для SSH
```sh
# ssh-keygen -f /root/.ssh/id_rsa
```
##### Добавить публичный ключ в authorized_keys на root@192.168.1.149
```sh
# ssh-copy-id -i /root/.ssh/id_rsa root@192.168.1.149
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.1.149's password:
Number of key(s) added: 1
Now try logging into the machine, with: "ssh 'root@192.168.1.149'"
and check to make sure that only the key(s) you wanted were added.
```

####  Установка кластера в конфигурации controller+worker с помощью k0sctl
> Пример конфигурационного файла k0sctl.yaml доступен в данном репозитории
```sh
# k0sctl apply --config k0sctl.yaml
⠀⣿⣿⡇⠀⠀⢀⣴⣾⣿⠟⠁⢸⣿⣿⣿⣿⣿⣿⣿⡿⠛⠁⠀⢸⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠀█████████ █████████ ███
⠀⣿⣿⡇⣠⣶⣿⡿⠋⠀⠀⠀⢸⣿⡇⠀⠀⠀⣠⠀⠀⢀⣠⡆⢸⣿⣿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀███          ███    ███
⠀⣿⣿⣿⣿⣟⠋⠀⠀⠀⠀⠀⢸⣿⡇⠀⢰⣾⣿⠀⠀⣿⣿⡇⢸⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠀███          ███    ███
⠀⣿⣿⡏⠻⣿⣷⣤⡀⠀⠀⠀⠸⠛⠁⠀⠸⠋⠁⠀⠀⣿⣿⡇⠈⠉⠉⠉⠉⠉⠉⠉⠉⢹⣿⣿⠀███          ███    ███
⠀⣿⣿⡇⠀⠀⠙⢿⣿⣦⣀⠀⠀⠀⣠⣶⣶⣶⣶⣶⣶⣿⣿⡇⢰⣶⣶⣶⣶⣶⣶⣶⣶⣾⣿⣿⠀█████████    ███    ██████████
k0sctl v0.15.0 Copyright 2022, k0sctl authors.
Anonymized telemetry of usage will be sent to the authors.
By continuing to use k0sctl you agree to these terms:
https://k0sproject.io/licenses/eula
INFO  ==> Running phase: Connect to hosts
INFO [ssh] 192.168.1.149:22: connected
INFO  ==> Running phase: Detect host operating systems
INFO [ssh] 192.168.1.149:22: is running CentOS Linux 7 (Core)
INFO  ==> Running phase: Acquire exclusive host lock
INFO  ==> Running phase: Prepare hosts
INFO  ==> Running phase: Gather host facts
INFO [ssh] 192.168.1.149:22: using localhost.localdomain as hostname
INFO [ssh] 192.168.1.149:22: discovered enp0s1 as private interface
INFO  ==> Running phase: Validate hosts
INFO  ==> Running phase: Gather k0s facts
INFO  ==> Running phase: Validate facts
INFO  ==> Running phase: Configure k0s
INFO [ssh] 192.168.1.149:22: validating configuration
INFO [ssh] 192.168.1.149:22: configuration was changed
INFO  ==> Running phase: Initialize the k0s cluster
INFO [ssh] 192.168.1.149:22: installing k0s controller
INFO [ssh] 192.168.1.149:22: waiting for the k0s service to start
INFO [ssh] 192.168.1.149:22: waiting for kubernetes api to respond
INFO  ==> Running phase: Release exclusive host lock
INFO  ==> Running phase: Disconnect from hosts
INFO  ==> Finished in 4m28s
INFO k0s cluster version 1.25.4+k0s.0 is now installed
INFO Tip: To access the cluster you can now fetch the admin kubeconfig using:
INFO  k0sctl kubeconfig
```
##### Создать символическую ссылку на k0s после успешной установки
```sh
# ln -s /usr/local/bin/k0s /bin/k0s
```
##### Создать алиас для k0s kubectl
```sh
# echo "alias kubectl='k0s kubectl'" >> /root/.bashrc
```
#### Проверка доступности кластера
#####  Статус кластера k0s
```sh
# k0s status
Version: v1.25.4+k0s.0
Process ID: 5379
Role: controller
Workloads: true
SingleNode: false
Kube-api probing successful: true
Kube-api probing last error:
```
```
# kubectl cluster-info
Kubernetes control plane is running at https://localhost:6443

CoreDNS is running at https://localhost:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
	
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
##### Список всех запущенных Pod
```
# kubectl get pods -A
NAMESPACE     NAME                              READY   STATUS    RESTARTS      AGE
kube-system   coredns-5d5b5b96f9-lcndt          1/1     Running   0             51m
kube-system   konnectivity-agent-cqrjv          1/1     Running   0             51m
kube-system   kube-proxy-s7hlc                  1/1     Running   0             51m
kube-system   kube-router-qhrbg                 1/1     Running   0             51m
kube-system   metrics-server-69d9d66ff8-4lxhx   1/1     Running   1 (46m ago)   51m
```
#### Установка NGINX Ingress Controller
> Пример YAML-файла с манифестами ingress-nginx.yaml доступен в данном репозитории
```
# kubectl apply -f ingress-nginx.yaml
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
serviceaccount/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
configmap/ingress-nginx-controller created
service/ingress-nginx-controller created
service/ingress-nginx-controller-admission created
deployment.apps/ingress-nginx-controller created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
ingressclass.networking.k8s.io/nginx created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
```
#### Установка примера сервиса
#####  Создание Namespace
```
# kubectl create namespace example-nginx
namespace/example-nginx created
```
#####  Создание Deployment 
```
# kubectl -n example-nginx apply -f example-nginx-application/deployment.yaml
deployment.apps/nginx-example-deployment created
```
#####  Создание Service 
```
kubectl -n example-nginx apply -f example-nginx-application/service.yaml
service/nginx-example-service created
```
#####  Создание Ingress 
```
# kubectl -n example-nginx apply -f example-nginx-application/ingress.yaml
ingress.networking.k8s.io/example-nginx-ingress created
```
