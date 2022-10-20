# Playbook for automating ad-guard docker version


## Contents for the ansible playbook.yaml file
```yml
- name: Setting up Adguard-home
  hosts: <hosts or host of where the playbook is meant to be executed at>
  become: true
  tasks:

    - name: Refreshing cache
      command: sudo apt update

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

    - name: Creating mount folder for the container
      file:
        path: /etc/adguard-home/conf
        state: directory

    - name: Creating mount folder for the container
      file:
        path: /etc/adguard-home/work
        state: directory

    - name: Stopping systemd-resolved
      service: 
        name: systemd-resolved
        state: stopped
        enabled: no

    - name: Setting up the Docker container...
      docker_container:
        name: Adgurad-home
        image: adguard/adguardhome
        volumes:
          - /etc/adguard-home/conf/:/opt/adguardhome/conf
          - /etc/adguard-home/work/:/opt/adguardhome/work
        state: started
        restart: yes
        detach: yes
        ports:
         - "53:53/tcp"
         - "53:53/udp"
         #- "67:67/udp" remove if you want to use adguard-home as a DHCP server
         #- "68:68/udp" remove if you want to use Adguard-home as a DHCP server
         - "80:80/tcp"
         - "443:443/tcp" 
         - "443:443/udp"
         - "3000:3000/tcp"
         - "853:853/tcp"
         - "784:784/udp"
         - "853:853/udp"
         - "8853:8853/udp"
         - "5443:5443/tcp"
         - "5443:5443/udp"
```


## Run following command to execute the ansible playbook
```bash
ansible-playbook name-of-deploymentfile.yml
```
