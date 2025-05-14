## Create VM
```bash
incus launch images:ubuntu/22.04/cloud VM-NAME --vm --device root,size=30GiB --storage STORAGE-POOL-NAME
```

## Create Container
```bash
incus launch images:ubuntu/22.04/cloud CONTAINER-NAME --storage STORAGE-POOL-NAME
```