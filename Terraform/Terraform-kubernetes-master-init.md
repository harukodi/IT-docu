# Initiate a kubernetes master on digital ocean with terraform


## Create a file .tf file example main.tf
```HCL
terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "2.22.3"
    }
  }
}


variable "do_token" {} #Your token from digital-ocean
variable "pvt_key" {} #Private key
variable "K3Stoken" {} #Token for your k3s cluster


data "http" "public_ip" {
  url = "https://ifconfig.me"
}


provider "digitalocean" {
  token = var.do_token
}


resource "digitalocean_ssh_key" "xia1997xsshKey" {
  name       = "xia1997x-id_rsa-pub"
  public_key = file("/home/xia1997x/.ssh/id_rsa.pub")
}


resource "digitalocean_droplet" "droplet-kube-master-naster" {
  image  = "ubuntu-20-04-x64"
  name   = "droplet-kube-master-naster"
  region = "fra1"
  size   = "s-1vcpu-1gb"
  ssh_keys = [digitalocean_ssh_key.xia1997xsshKey.fingerprint]
}


resource "digitalocean_firewall" "droplet-kube-master-naster" {
  name = "kubernetes-ports"

  droplet_ids = [digitalocean_droplet.droplet-kube-master-naster.id]

  inbound_rule {
    protocol         = "tcp"
    port_range       = "22"
    source_addresses = ["${data.http.public_ip.response_body}"]
  }

  inbound_rule {
    protocol         = "tcp"
    port_range       = "6443"
    source_addresses = ["0.0.0.0/0", "::/0"]
  }


  inbound_rule {
    protocol         = "tcp"
    port_range       = "2379"
    source_addresses = ["0.0.0.0/0", "::/0"]
  }

  inbound_rule {
    protocol         = "tcp"
    port_range       = "2380"
    source_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "2380"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "2379"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "6443"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "443"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "80"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "53"
    destination_addresses = ["0.0.0.0/0", "::/0"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "22"
    destination_addresses = ["${data.http.public_ip.response_body}"]
  }

  connection {

    type     = "ssh" # Connection type for the connection

    user     = "root" # User to use when establishing the connection

    private_key = file(var.pvt_key) # Your private ssh-key

    host = digitalocean_droplet.droplet-kube-master-naster.ipv4_address # Fetch ip after the droplet is created

    timeout = "1m" # Timeout for the retrying sequence when connection fails

  }



# Provision the droplet to initiate a cluster for k3s

  provisioner "remote-exec" {

    inline = [

      "apt update -y",
      "apt install git -y",
      "git clone https://github.com/harukodi/bashscripts.git",
      "cd bashscripts",
      "sh k3s-server-init-terraform.sh ${var.K3Stoken}",
      "kubectl get nodes"

    ]
  }
}
```


## Create a tfvars file example terraform.tfvars
```yml
do_token = "token-for-digitalocean-goes-here"

pvt_key = "path-to-private-ssh-key-goes-here"

K3Stoken = "token-for-the-kubernetes-cluster"
```


## Run the terraform file
```bash
terraform apply -auto-approve
```

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.digitaloceanspaces.com/WWW/Badge%203.svg)](https://www.digitalocean.com/?refcode=1f77b7c1dff3&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)


