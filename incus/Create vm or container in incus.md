## Create a container
```bash
incus launch images:ubuntu/22.04 container-name
```
## Create a vm
```bash
incus launch images:ubuntu/22.04 vm-name --vm
```
## Get a shell in your vm/container
```
incus exec container-name/vm-name -- bash
```