```yaml
network:
    version: 2
    ethernets:
        enp1s0:
            dhcp4: true
            dhcp4-overrides:
              use-dns: false
            nameservers:
              addresses: [8.8.8.8,8.8.4.4]
```

## Apply with the netplan command
```bash
sudo netplan apply
```