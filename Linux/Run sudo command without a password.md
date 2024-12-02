## Open the sudoers file
```bash
sudo visudo
```

## Add the following to the end of the file
#### Example one with commands
```
username ALL=(ALL) NOPASSWD: /usr/bin/apt, /usr/bin/whoami
```
#### Example two without commands
```
username ALL=(ALL) NOPASSWD: /usr/bin/command, /usr/bin/command2
```

## Allow everyone of group sudo to run commands without password
```bash
%sudo ALL=(ALL:ALL) NOPASSWD: ALL
```