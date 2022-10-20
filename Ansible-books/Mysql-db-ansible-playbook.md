# Playbook for mysql-db


## Contents for the ansible playbook.yaml file
```yml
- name: install mysql
  hosts: <hosts or host of where the playbook is meant to be executed at>
  vars:
    db_user: test_user #db_user for the database
    db_pass: test_pass #db_pass password for the database
    service_account: root #Service_account account to use when running this book, can be omitted if using a private key
    service_pass: labbansible #Service_pass password for the Service_account, can be omitted if using a private key
    db_name: testdb #db_name the name of the database
  become: true
  tasks:
    - name: Ansible apt install mysql
      apt:
        pkg:
        -  mysql-server
        -  python-mysqldb
        state: latest

    - name: Start Mysql service
      command: systemctl start mysql.service

    - name: check status of mysql_version!
      command: mysql --version
      register: mysql_version
    - debug: var=mysql_version.stdout_lines

    - name: status of mysql service
      command: systemctl status mysql
      register: mysql_service_status
    - debug: var=mysql_service_status.stdout_lines

    - name: Create a new database
      mysql_db:
        name: "{{ db_name }}"
        state: present
        login_password: "{{ service_pass }}"
```


## Run following command to execute the ansible playbook
```bash
ansible-playbook name-of-deploymentfile.yml -e 'ansible_python_interpreter=/usr/bin/python3'
```
