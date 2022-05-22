# Домашнее задание к занятию "8.4 Работа с Roles"

### Важно!!!
По [ссылке](https://github.com/netology-code/mnt-homeworks/tree/MNT-13/08-ansible-04-role) можно найти обновленное домашнее задание к занятию "Работа с Roles".
В этом задании используется другой стек технологий, позволяющий заменить ELK, в связи с запретом на установку стека ELK из официальных источников.
Все новые продукты являются аналогами старых, что позволяет не только безболезненно выполнить задания, но и познакомиться с возможными будущими заменами в стандартном ELK стеке внутри той компаниий, где вы будете работать.
Желаем успехов в выполнении задания!

## Подготовка к выполнению
1. Создайте два пустых публичных репозитория в любом своём проекте: kibana-role и filebeat-role.
2. Добавьте публичную часть своего ключа к своему профилю в github.

## Основная часть

Наша основная цель - разбить наш playbook на отдельные roles. Задача: сделать roles для elastic, kibana, filebeat и написать playbook для использования этих ролей. Ожидаемый результат: существуют два ваших репозитория с roles и один репозиторий с playbook.

1. Создать в старой версии playbook файл `requirements.yml` и заполнить его следующим содержимым:
   `
   ---
     - src: git@github.com:netology-code/mnt-homeworks-ansible.git
       scm: git
       version: "2.1.4"
       name: elastic 
   `
   `[root@localhost roles]# ls
     requirements.yml`
   
2. При помощи `ansible-galaxy` скачать себе эту роль.                          `
[root@localhost roles]# ansible-galaxy install -r requirements.yml --roles-path ./
The authenticity of host 'github.com (140.82.121.3)' can't be established.
ECDSA key fingerprint is SHA256:p2QAMXNIC1TJYWeI.
ECDSA key fingerprint is MD5:7b:99:81:1e:4c:91:a5:0d:5a:2e:2e:80:13:3f:24:ca.
Are you sure you want to continue connecting (yes/no)? yes
- extracting elastic to /home/alaricode/test/roles/elastic
- elastic (2.0.0) was installed successfully 
`
3. Создать новый каталог с ролью при помощи `ansible-galaxy role init kibana-role`.                                                  
`
[root@localhost roles]# ansible-galaxy role init kibana-role
- Role kibana-role was created successfully
[root@localhost roles]# ls
elastic  kibana-role  requirements.yml
`
4. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`.
```yml
[root@localhost kibana-role]# nano defaults/main.yml
[root@localhost kibana-role]# nano vars/main.yml     
```
5. Перенести нужные шаблоны конфигов в `templates`. `готово`
6. Создать новый каталог с ролью при помощи `ansible-galaxy role init filebeat-role`.   
```
[root@localhost roles]# ls
elastic  kibana-role  requirements.yml
[root@localhost roles]# ansible-galaxy role init filebeat-role
- Role filebeat-role was created successfully
[root@localhost roles]# ls
elastic  filebeat-role  kibana-role  requirements.yml 
```
7. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`. 
```
[root@localhost filebeat-role]# ls
defaults  files  handlers  meta  README.md  tasks  templates  tests  vars  
```
8. Перенести нужные шаблоны конфигов в `templates`.  
```
[root@localhost test]# ls roles/kibana-role/templates/
kibana.sh.j2  kibana.yml.j2
[root@localhost test]# ls roles/filebeat-role/templates/
filebeat.sh.j2  filebeat.yml.j2
[root@localhost test]#   
```
9. Описать в `README.md` обе роли и их параметры. 
```
[README.md](https://github.com/pnadezhdin/ansible-roles/blob/main/filebeat-role/README.md)  [README.md](https://github.com/pnadezhdin/ansible-roles/blob/main/kibana-role/README.md)  
```

10. Выложите все roles в репозитории. Проставьте тэги, используя семантическую нумерацию.                                            

11. Добавьте roles в `requirements.yml` в playbook.
```
[requirements.yml](https://github.com/pnadezhdin/ansible-roles/blob/main/requirements.yml)

---
  - src: git@github.com:netology-code/mnt-homeworks-ansible.git
    scm: git
    version: "2.1.4"
    name: elastic
  - src: git@github.com:run0ut/kibana-role.git
    scm: git
    version: "1.1.0"
    name: kibana
  - src: git@github.com:run0ut/filebeat-role.git
    scm: git
    version: "1.1.0"
    name: filebeat
    
 
- name: Get IPs
  hosts: all
  pre_tasks:
    - name: Hosts IP adreses
      ansible.builtin.setup:
        filter:
          - default_ipv4
      tags: always

- name: Install Elasticsearch
  hosts: elasticsearch
  roles:
    - elastic
  tags: elastic

- name: Install Kibana
  hosts: kibana
  roles:
    - kibana
  tags: kibana

- name: Install Filebeat
  hosts: filebeat
  roles:
    - filebeat
  tags: filebeat

```
12. Переработайте playbook на использование roles.
`requirements.yml`
```
---
- name: Get ips
  hosts: all
  pre_tasks:
    - name: Hosts IP adresses 
      ansible.builtin.setup:
        filter:
          - default_ipv4
      tags: ipv4

- name: Install Elasticsearch
  hosts: elasticsearch
  roles:
    - elastic
  tags: elastic

- name: Install Kibana
  hosts: kibana
  roles:
    - kibana
  tags: kibana

- name: Install Filebeat
  hosts: filebeat
  roles:
    - filebeat
  tags: filebeat

 ``` 
13. Выложите playbook в репозиторий.
`[playbook.yml](https://github.com/pnadezhdin/ansible-roles/blob/main/playbook.yml)`

14. В ответ приведите ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.

```  
[kibana-role](https://github.com/pnadezhdin/kibana-role)

[filebeat-role](https://github.com/pnadezhdin/filebeat-role)

[playbook.yml](https://github.com/pnadezhdin/ansible-roles/blob/main/playbook.yml) 
```
## Необязательная часть

1. Проделайте схожие манипуляции для создания роли logstash.
2. Создайте дополнительный набор tasks, который позволяет обновлять стек ELK.
3. Убедитесь в работоспособности своего стека: установите logstash на свой хост с elasticsearch, настройте конфиги logstash и filebeat так, чтобы они взаимодействовали друг с другом и elasticsearch корректно.
4. Выложите logstash-role в репозиторий. В ответ приведите ссылку.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
