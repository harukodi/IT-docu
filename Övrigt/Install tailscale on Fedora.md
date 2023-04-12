## Add the Tailscale repo
```bash
sudo dnf config-manager --add-repo https://pkgs.tailscale.com/stable/fedora/tailscale.repo
```

## Install tailscale with dnf
```bash
sudo dnf install tailscale
```

## Enable tailscale with systemctl
```bash
sudo systemctl enable --now tailscaled
```

## Connect your Tailscale node to your Tailscale network
```bash
sudo tailscale up
```

## Set enforcement to permissive to allow tailscale ssh to work(Optional)
```bash
setenforce permissive
```

## Change enforcement mode permanently

## Edit /etc/sysconfig/selinux and change SELINUX to permissive

```bash
nano /etc/sysconfig/selinux
```

**EXAMPLE**
```bash
# vi /etc/sysconfig/selinux
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=permissive
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```