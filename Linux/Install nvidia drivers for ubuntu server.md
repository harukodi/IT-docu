## **NOTE:**
Anywhere you see `470` in the examples or commands, make sure to replace it with the version number of your driver that you want to install.
## Step 1: Check your current driver version
```bash
cat /proc/driver/nvidia/version
```
**EXAMPLE OUTPUT:**
```
NVRM version: NVIDIA UNIX x86_64 Kernel Module  >470<|this is your driver version|.256.02  Thu May  2 14:37:44 UTC 2024
GCC version:  gcc version 12.3.0 (Ubuntu 12.3.0-1ubuntu1~22.04)
```
---
## Step 2: Remove old nvidia drivers
```bash
sudo apt --purge remove '*nvidia*470*'
```
---
## Step 3: Check the available drivers for your hardware
```bash
sudo ubuntu-drivers list --gpgpu
```
---
## Step 4: Installing the drivers on the server
```bash
sudo ubuntu-drivers install --gpgpu nvidia:470-server
```
---
## Step 5: Install needed components for the driver
```bash
sudo apt install nvidia-utils-470-server
```
---
## Step 6: Install the driver DKMS package for your driver
```bash
sudo apt install nvidia-dkms-470-server
```
---
## Step 7: reboot your server
```bash
sudo reboot now
```