**IMPORTANT:**  "_For security reasons, it's recommended to remove any port forwards for the server during this process. Since your root password will be temporarily set to something easy to guess, removing port forwards reduces the risk of unauthorized access._"
___

## Step 1: Change Root Password
Before beginning the migration process from your old motherboard to the new one, it's advisable to change your root password to something easy to remember. After completing the migration, remember to change it back to a stronger password.

## Step 2: Backup Your Server (Optional but Recommended)
Consider making a backup of your Ubuntu server before proceeding with the motherboard swap. This ensures you have a copy of your data in case anything goes wrong during the migration.

## Step 3: Login to Your Server
Ensure you have physical access to your Ubuntu server and log in using your username and password.

## Step 4: Identify New Interface Name
After swapping the motherboard, it's essential to identify the new interface name. You can do this by running the following command:
```bash
nmcli device status
```
In this example, let's assume the new interface name is "enp7s0".

## Step 5: Modify Netplan Configuration
Navigate to the `/etc/netplan/` directory and locate the YAML file that needs to be modified. In this case, let's assume it's named "00-installer-config.yaml". Edit the file and replace `"your-new-interface-name-here"` with the actual interface name discovered in the previous step. Save your changes.

**Example configuration:**
```yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    your-new-interface-name-here:
      dhcp4: true
      nameservers:
        addresses: [76.76.2.199, 76.76.10.199]
  version: 2
```

**Example configuration in this case:**
```yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp7s0:
      dhcp4: true
      nameservers:
        addresses: [76.76.2.199, 76.76.10.199]
  version: 2
```

## Step 6: Apply Netplan Changes
To apply the changes made to the Netplan configuration, execute the following commands:
```bash
sudo netplan generate; sudo netplan apply
```
These commands regenerate and apply the network configuration respectively.

##### That's it! Your network interface should now be up and running with the new motherboard. If it doesn't work immediately, consider restarting your server. :)