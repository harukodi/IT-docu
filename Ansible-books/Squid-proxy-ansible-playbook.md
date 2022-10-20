# Playbook for a http Squid proxy


## Contents for the ansible playbook.yaml file
```yml
- name: Squid-proxy-deployment
  hosts: <hosts or host of where the playbook is meant to be executed at>
  become: true
  vars_prompt:
    - name: username
      prompt: Enter Username
      private: no
      unsafe: yes
    - name: password
      prompt: Enter Password
      private: yes
      unsafe: yes
    - name: squidProxyPort
      prompt: Enter port
      private: no
      unsafe: yes
  tasks:    
   
   
    - name: Installing Squid-proxy and apache2-utils
      apt:
       pkg:
       - apache2-utils
       - squid
       state: latest


    - name: Status of squid service
      command: systemctl status squid.service
      register: sytemctl_status_squid
    - debug: var=sytemctl_status_squid


    - name: Removing default config for squid
      file:
        path: /etc/squid/squid.conf
        state: absent


    - name: Setting up predefined variables for squid
      copy:
        dest: /etc/squid/squid.conf
        content: |
          auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwords
          auth_param basic realm squid-proxy-password-username-required
          acl authenticated proxy_auth REQUIRED
          http_access allow authenticated
          include /etc/squid/conf.d/*
          http_port {{ squidProxyPort }}


    - name: Securing password with blowfish-algorithm
      command: htpasswd -c -B -b /etc/squid/passwords {{ username }} {{ password }}


    - name: Restarting service squid
      command: systemctl restart squid.service
```


## Run following command to execute the ansible playbook
```bash
ansible-playbook name-of-deploymentfile.yml -e 'ansible_python_interpreter=/usr/bin/python3'
```
