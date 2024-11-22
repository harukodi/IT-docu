
# List
## Only list containers
```bash
incus list type=CONTAINER
```
## Only list vms
```bash
incus list type=VIRTUAL-MACHINE
```

# Container/vm config

## Change cpu and memory limits
```bash
incus config set container-name/vm-name limits.cpu=4 limits.memory=4GiB
```

## Enlarge vm volume :
```bash
incus config device override vm-name root size=50GiB
```
## Get a shell in your vm/container
```
incus exec container-name/vm-name -- bash
```
or
```
incus shell container-name/vm-name
```