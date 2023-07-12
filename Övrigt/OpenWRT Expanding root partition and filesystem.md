## Step 1 type the following in the command line of openWRT use SSH
*NOTE*: Copy the commands one at a time in to the terminal
```bash
# Install packages
opkg update
opkg install parted
 
# Identify disk name and partition number
echo -e "ok\nfix" | parted -l ---pretend-input-tty
 
# Expand root partition
parted -s /dev/sda resizepart 2 100%
 
# Apply changes
reboot
```
## Step 2 type the following in the command line of openWRT use SSH
*NOTE*: Copy the commands one at a time in to the terminal
```bash
# Install packages
opkg update
opkg install losetup resize2fs
 
# Map loop device to root partition
losetup /dev/loop1 /dev/sda2
 
# Expand root filesystem
resize2fs -f /dev/loop1
 
# Apply changes
reboot
```
## Now you have expanded your root partition and filesystem