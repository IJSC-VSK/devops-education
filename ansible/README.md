Ansible

#### Запуск используя Docker
```sh
$ git clone https://gitlab.vsk.ru/marlin/tutorials/devops-basics.git
$ docker run -v /home/eldar/devops-basics/ansible/:/ansible -it artifactory.vsk.ru/marlin-platform/ansible:2.10 sh
```

### Примеры
Где:
- `-k` запрос пароля при ssh подключении к целевым хостам
- `-K` запрос пароля для sudo задач

#### Пример запуска
```
$ cd /ansible
$ ansible-playbook -i inventory/devops-vm.yml playbooks/devops.yml -u eldar -k -K
```
