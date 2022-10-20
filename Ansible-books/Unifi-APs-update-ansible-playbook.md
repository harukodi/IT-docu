# Playbook for automating Unifi APs to a new version


## Contents for the ansible playbook.yaml file
```yml
- name: autoUP-AP
  hosts: <hosts or host of where the playbook is meant to be executed at>
  vars_prompt:
    - name: updateURL # Update URL for the .bin file
      prompt: Sir/Mrs please Enter URL # Input URL to .bin file for the update
      unsafe: yes # Allows unsafe characters example /
      private: no
  tasks:
    - name: Updating the AP/S
      raw: upgrade {{ updateURL }}
      register: upgrade_process
    - debug: var=upgrade_process.stdout_lines
```


## Run following command to execute the ansible playbook
```bash
ansible-playbook name-of-deploymentfile.yml
```
