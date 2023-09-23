
## Step 1 Remove local-lvm in the Proxmox GUI
> #### Datacenter > Storage > local-lvm > Remove

## Step 2 run the following commands in the Proxmox shell

### Command 1
```bash
lvremove /dev/pve/data
```

### Command 2
```bash
lvresize -l +100%FREE /dev/pve/root
```

### Command 3
```bash
resize2fs /dev/mapper/pve-root
```

## Optional step to allow CT creation on the local disk
> #### Datacenter > Storage > Local > Edit > Content > Container > Ok
# Done :)