## Use wget to fetch a cloud image
```bash
wget "https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img"
```

## Add a serial console to the reference VM
```
qm set <VM ID> --serial0 socket --vga serial0
```

## Change the file extension of the image to .qcow2
```
mv jammy-server-cloudimg-amd64.img jammy-server-cloudimg-amd64.qcow2
```

## Resize the downloaded cloud image
```
qemu-img resize jammy-server-cloudimg-amd64.qcow2 32G
```

## Import the cloud image into Proxmox
```
qm importdisk <VM ID> jammy-server-cloudimg-amd64.qcow2 local
```
## If the above dosen't work try this command
```
qm importdisk <VM ID> jammy-server-cloudimg-amd64.qcow2 local-lvm
```