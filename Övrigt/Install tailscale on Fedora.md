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
