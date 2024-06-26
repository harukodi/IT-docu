## Setup LUKS with Cryptsetup
>
>**NOTE:** the part "/dev/sdX" may be different for you!
>
#### Use this command to encrypt a drive
```bash
sudo cryptsetup luksFormat --type luks2 /dev/sdX
```

#### Use this command to create a mapping for the device "/dev/sdX"
```bash
sudo cryptsetup luksOpen /dev/sdX sdX
```

#### Use this command to create a new filesystem on "/dev/mapper/sdX"
```bash
sudo mkfs.ext4 /dev/mapper/sdX
```

## Open/mount the encrypted drive/partition
#### Use the following command to open the encrypted partition
```bash
sudo cryptsetup luksOpen /dev/sdX sdX
```

#### Use the following to mount the encrypted partition
```bash
mkdir sdX; mount /dev/mapper/sdX sdX
```
