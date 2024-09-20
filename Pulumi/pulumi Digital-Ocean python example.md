## Start a project with Pulumi
```bash
pulumi new
```
**NOTE:** Choose template then choose "digitalocean-python"
## Set your doToken for digitalocean
```bash
pulumi config set digitalocean:token your-token-goes-here --secret
```

## Pulumi code example to create a droplet on digitalocean
```py
import pulumi
import pulumi_digitalocean as digitalocean

ssh_key_fingerprint=["your-ssh-key-fingerprint-goes-here"]
user_data = '''#!/bin/bash
sudo apt update; sudo apt install nginx -y
'''

def create_droplets(count):
    for digital_ocean_droplet in range(count):
        droplet = digitalocean.Droplet(f"web-{digital_ocean_droplet}",
            image="ubuntu-20-04-x64",
            name=f"web-{digital_ocean_droplet}",
            region=digitalocean.Region.FRA1,
            user_data=user_data,
            ssh_keys=ssh_key_fingerprint,
            size=digitalocean.DropletSlug.DROPLET_S1_VCPU1_GB)
        pulumi.export(f"web-{digital_ocean_droplet}", droplet.ipv4_address)

if __name__ == "__main__":
    create_droplets(1)
```

## Create your pulumi stack
```bash
pulumi up
```