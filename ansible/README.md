Ansible

#### Запуск используя Docker
```sh
$ git clone https://gitlab.vsk.ru/marlin/tutorials/devops-basics.git /tmp/devops-basics
$ chown -R 1001:1001 /tmp/devops-basics/
$ docker run -v /home/eldar/devops-basics/ansible/:/ansible -it repo.vsk.ru:5000/marlin-platform/ansible:2.10 sh
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
