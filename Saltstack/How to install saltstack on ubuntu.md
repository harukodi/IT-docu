
## 1. Add the Salt Project repository key and create the apt sources list file
```bash
sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg [https://repo.saltproject.io/salt/py3/ubuntu/22.04/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg](https://repo.saltproject.io/salt/py3/ubuntu/22.04/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg)

echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.gpg arch=amd64] [https://repo.saltproject.io/salt/py3/ubuntu/22.04/amd64/latest](https://repo.saltproject.io/salt/py3/ubuntu/22.04/amd64/latest) jammy main" | sudo tee /etc/apt/sources.list.d/salt.list
```

## 2. apt-update
```bash
sudo apt update
```

## 3. Install the salt components
```bash
sudo apt-get install salt-master
sudo apt-get install salt-minion
sudo apt-get install salt-ssh
```
---
**NOTE:**
### You don't need to install the salt master if you intend to use a machine or a resource as a salt-minion.
### You don't need to install the salt minion if you intend to use a machine or a resource as a salt-master.
---

## 4. Enable and start the salt services
```bash
sudo systemctl enable salt-master && sudo systemctl start salt-master
sudo systemctl enable salt-minion && sudo systemctl start salt-minion
```
---
**NOTE:**
### If you just installed one of the Salt components be sure to enable and start the right service.
---
###### Done :) next step is configuring the saltstack minion and the master!


## 作者: Xia1997x
## Written by Xia1997x