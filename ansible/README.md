Ansible

### Сборка и запуск
```
$ git clone https://github.com/IJSC-VSK/devops-education.git /tmp/devops-basics
$ cd /tmp/devops-basics
$ docker build . -t ansible
$ docker run -v /tmp/devops-basics/ansible:/ansible -it ansible sh
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
