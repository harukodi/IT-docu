# Initiate client connection to a tailscale network on digital ocean


## Create a file .tf file example main.tf
```HCL
# The provider terraform should use in this case it's Digitalocean
# Tailscale-demo = Droplet ID
# Default location for the ssh-key/ssh-key pub {~/.ssh/}
# Default location for the ssh-key-private {~/.ssh/id_rsa}
# Default location for the ssh-key-public {~/.ssh/id_rsa.pub}
# You may need to generate an SSH-key with the command {ssh-keygen -t rsa}
terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "1.22.2"
    }
  }
}

variable "do_token" {
  type = string
  default = "token-key-goes-here"
  description = "token-key-digital-ocean"
}

variable "pvt_key" {
  description = "private-ssh-key"
}

variable "tailscaleKey" {
  type = string
  default = "tailscale-auth-key-goes-here"
  description = "tailscale-auth-key"
}


# Configure the DigitalOcean Provider.
provider "digitalocean" {
  token = var.do_token
}

# Create ssh-key
resource "digitalocean_ssh_key" "terraform-ssh-key" {
  name       = "terraform-ssh-key"
  public_key = file("~/.ssh/id_rsa.pub")
}

# Create a resource in this example a droplet on Digitalocean.
resource "digitalocean_droplet" "tailscale-demo"{
  image  = "ubuntu-20-04-x64"
  name   = "tailscale-1-demo"
  region = "fra1"
  size   = "s-1vcpu-1gb"
  private_networking = true
  ssh_keys = [digitalocean_ssh_key.terraform-ssh-key.fingerprint]
}

# Create the Firewall for the droplet and opening the most common ports.
  resource "digitalocean_firewall" "tailscale-demo" {
  name = "tailscale-demo-firewall"

  droplet_ids = [digitalocean_droplet.tailscale-demo.id]

  inbound_rule {
    protocol         = "tcp"
    port_range       = "22"
    source_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol         = "tcp"
    port_range       = "22"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol         = "tcp"
    port_range       = "80"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol         = "tcp"
    port_range       = "443"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol         = "icmp"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "53"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

# Create the connection for the remote-exec provisioner.
  connection {
    type     = "ssh" # Connection type for the connection
    user     = "root" # User to use when establishing the connection
    private_key = file(var.pvt_key) # Your private ssh-key
    host = digitalocean_droplet.tailscale-demo.ipv4_address # Fetch ip after the droplet is created
    timeout = "1m" # Timeout for the retrying sequence when connection fails
  }

# Provision the droplet to connect to a tailscale network.
  provisioner "remote-exec" {
    inline = [
      "curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null",
      "curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list",
      "sudo apt-get update",
      "sudo apt-get install tailscale -y",
      "tailscale up --authkey=${var.tailscaleKey}",
    ]
  }
}
```


## Run the terraform file
```bash
terraform apply -auto-approve
```

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.digitaloceanspaces.com/WWW/Badge%203.svg)](https://www.digitalocean.com/?refcode=1f77b7c1dff3&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)


