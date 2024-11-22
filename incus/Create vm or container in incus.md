## Create a container
```bash
incus launch images:ubuntu/22.04 container-name
```
## Create a vm
```bash
incus launch images:ubuntu/22.04 vm-name --vm
```

## Create a vm with custom size for disk
```bash
incus launch images:ubuntu/22.04 vm-name --vm --device root,size=30GiB
```