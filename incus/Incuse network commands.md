## Create a network forward
```bash
incus network forward create network-name listening-ip
```
## Remove a network forward
```bash
incus network forward delete network-name listening-ip
```

## Add port forward
```bash
incus network forward port add network-name listening-address tcp listening-port container-ip-or-vm-ip target-port
```

## Remove a port forward
```bash
incus network forward port remove network-name listening-address tcp listening-port
```
