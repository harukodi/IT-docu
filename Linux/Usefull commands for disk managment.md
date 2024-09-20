### The following command gives info about the drive e.g S/N
```bash
hdparm -i /dev/sdX
```

### The following command give you the UUID of a drive
```bash
blkid /dev/sdX
```

### The following command lists your drives
```bash
fdisk -l
```

### The "fdisk -l /dev/s*" command lists information about all disks starting with "/dev/s".
```bash
fdisk -l /dev/s*
```

### The following command tests your drive's read speed
```bash
sudo hdparm -Tt /dev/sdX
```

### The following command tests your drive's write speed
```bash
sync; dd if=/dev/zero of=tempfile bs=1M count=1024; sync
```

#### The following command creates a file system on your drive
#### **NOTE:** initializes a storage device or partition with a specified file system
```bash
sudo mkfs -t ext4 /dev/sdX
```
