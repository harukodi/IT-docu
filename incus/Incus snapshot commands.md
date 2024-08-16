## Create a snapshot for a vm or container
```bash
incus snapshot create container-name-vm-name snapshot-name
```
## Restore a snapshot
```bash
incus snapshot restore container-name-vm-name snapshot-name
```

## Delete a snapshot
```bash
incus snapshot delete -i container-name-vm-name snapshot-name
```
