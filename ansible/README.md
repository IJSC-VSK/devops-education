Ansible

#### Запуск используя Docker
```sh
$ git clone https://gitlab.vsk.ru/marlin/ansible.git
$ cd ansible/docker
$ docker run -it ansible-runtime:latest
```

### Примеры

Где:
- `-k` запрос пароля при ssh подключении к целевым хостам
- `-K` запрос пароля для sudo задач

#### Пример запуска роли bastion с использованием плейбука okd-install для обновления конфигурации BIND
```
$ ansible-playbook -i inventory/devops-vm.yml playbooks/devops.yml -u eldar -k -K
```
