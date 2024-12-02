## Create a file named base.yaml and copy the following
```yaml
config:
  cloud-init.user-data: |
    #cloud-config
    packages:
      - git
      - openssh-server
      - curl
    users:
    - name: USERNAME-GOES-HERE
      sudo: ALL=(ALL) NOPASSWD:ALL
      groups: sudo
      shell: /bin/bash
      lock_passwd: true
    runcmd:
      - ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
      - curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/jammy.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
      - curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/jammy.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list
      - sudo apt-get update
      - sudo apt-get install tailscale -y
      - tailscale up --auth-key tailscale-key-goes-here --ssh
description: Default Incus profile
devices:
  eth0:
    name: eth0
    network: incusbr0
    type: nic
  root:
    path: /
    pool: incus-storage-pool
    type: disk
name: base
project: default
```

## Start the incus container with this command
```bash
incus launch images:ubuntu/22.04/cloud CONTAINER-NAME < base.yaml
```