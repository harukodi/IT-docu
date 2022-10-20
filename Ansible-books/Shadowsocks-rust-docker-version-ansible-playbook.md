# Playbook for automating shadowsocks-rust docker version


## Contents for the ansible playbook.yaml file
```yml
- name: Setup Shadowsocks-rust with Docker
  hosts: <hosts or host of where the playbook is meant to be executed at>
  vars:
    ssport: 443 #Change this if you want the server to run on another port
    password: "" #Input the password you wish to use for the ssserver
    dns: 1.1.1.1 #Change this if you want the ssserver to use a different dns resolver
  become: true
  tasks:

    - name: Refreshing cache
      command: sudo apt-get update

    - name: Install aptitude
      apt:
        name: aptitude
        state: latest
    
    - name: Install required system packages
      apt:
        pkg:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
          - python3-pip
          - virtualenv
          - python3-setuptools
          - python-pip
        state: latest
        
    - name: Add Docker GPG apt Key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Refreshing cache
      command: sudo apt-get update

    - name: Add Docker Repository
      apt_repository:
        repo: deb https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Refreshing cache
      command: sudo apt-get update

    - name: Update apt and install docker-ce
      apt:
        name: docker-ce
        state: latest
        
    - name: Install Docker Module for Python
      pip:
        name: docker

    - name: pulling image of shadowsocks-rust-server
      docker_image:
        name: ghcr.io/shadowsocks/ssserver-rust:latest

    - name: Creating mount folder for the container
      file:
        path: /etc/shadowsocks-rust-data
        state: directory

    - name: Generating config.json file for the container
      copy:
        content: ""
        dest: /etc/shadowsocks-rust-data/config.json
        force: no

    - name: Setting up predefinde variables
      copy:
        dest: /etc/shadowsocks-rust-data/config.json
        content: |
          {
            "server": "0.0.0.0",
            "server_port": 8388,
            "password": "{{ password }}",
            "method": "aes-256-gcm",
            "mode": "tcp_only",
            "protocol": "dns",
            "local_address": "0.0.0.0",
            "local_port": 53,
            "local_dns_port": 53,
            "remote_dns_address": "{{ dns }}",
            "remote_dns_port": 53,
            "no_delay": true,
          }      

    - name: Setting up the Docker container...
      docker_container:
        name: Shadowsocks-rust
        image: ghcr.io/shadowsocks/ssserver-rust:latest
        volumes:
          - /etc/shadowsocks-rust-data/config.json:/etc/shadowsocks-rust/config.json
        state: started
        restart: yes
        ports:
         - "{{ ssport }}:8388/tcp"
```


## Run following command to execute the ansible playbook
```bash
ansible-playbook name-of-deploymentfile.yml
```
