
# To get started generate ssh-keys with the following names "gcp-ssh-key.pub" and "gcp-ssh-key"


## Use the following command to generate the rsa-keys with the names provided above
```bash
ssh-keygen -b 2048 -t rsa -f gcp-ssh-key -N ""
```

## main.tf
```HCL
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.43.1"
    }
    random = {
      source = "hashicorp/random"
      version = "3.4.3"
    }
  }
}

variable "project_id" {}
variable "region" {}
variable "zone" {}
variable "k3s_token" {}
variable "nodecount" {}
variable "gcp_key_file" {}
variable "tailscale_key" {}
variable "ssh_username" {}
variable "ssh-pub-key" {}

provider "google" {
  project     = var.project_id
  region      = var.region
  credentials = file(var.gcp_key_file)
}


resource "google_compute_address" "static" {
  name = "external-ipv4-address"
}


resource "google_compute_instance" "default" {
  name         = "learning-gcp-instance-1-k3s-master"
  machine_type = "g1-small"
  zone         = var.zone
  tags = ["learning-gcp-k3s"]

  boot_disk {
    initialize_params {
      image = "ubuntu-os-cloud/ubuntu-1804-lts"
      labels = {
        my_label = "value"
      }
    }
  }

  network_interface {
    network = "default"

    access_config {
      nat_ip = google_compute_address.static.address
    }
  }

  metadata = {
    ssh-keys = "${var.ssh_username}:${file(var.ssh-pub-key)}"
  }

  metadata_startup_script = <<SCRIPT
    sudo apt update
    sudo apt install git -y
    git clone https://github.com/harukodi/bashscripts.git
    cd bashscripts
    sudo sh k3s-server-init-terraform.sh ${var.k3s_token}
    curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
    curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list
    sudo apt-get update
    sudo apt-get install tailscale -y
    sudo tailscale up --authkey=${var.tailscale_key}
    sudo tailscale up --auth-key=${var.tailscale_key} --ssh
  SCRIPT


}


resource "google_compute_firewall" "default" {
  name    = "allow-k3s-port"
  network = "default"

  allow {
    protocol = "tcp"
    ports    = ["6443"]
  }

  allow {
    protocol = "tcp"
    ports    = ["2380"]
  }

  allow {
    protocol = "tcp"
    ports    = ["2379"]
  }

  source_ranges = ["0.0.0.0/0"]
  target_tags = ["learning-gcp-k3s"]
}


output "instance_ips" {
  value = google_compute_address.static.address
}


resource "google_compute_instance" "k3s-slave" {
  count = var.nodecount
  name         = "learning-gcp-instance-k3s-slave-${count.index + 1}"
  machine_type = "g1-small"
  zone         = var.zone
  tags = ["learning-gcp-k3s"]

  boot_disk {
    initialize_params {
      image = "ubuntu-os-cloud/ubuntu-1804-lts"
      labels = {
        my_label = "value"
      }
    }
  }

  network_interface {
    network = "default"

    access_config {
    }
  }

  metadata_startup_script = <<SCRIPT
    #!/bin/bash
    sudo apt update
    sudo apt install git -y
    git clone https://github.com/harukodi/bashscripts.git
    cd bashscripts
    sudo sh k3s-cluster-join-terraform.sh "${var.k3s_token}" "${google_compute_address.static.address}"
  SCRIPT

depends_on = [google_compute_instance.default]

}
```


## terraform.tfvars
```
project_id = "project-id-goes-here"
region = "region-goes-here"
zone = "zone-in-the-region-goes-here"
gcp_key_file = "gcp-key.json" # Name should stay the same "gcp-key.json" that's the default name when downloading your key from gcp
k3s_token = "k3s-token-goes-here" # Choose your token you want for k3s. Omit "$" in your token or the process will fail
nodecount = 4 # Must atleast be 2 or more nodes in the cluster
ssh_username = "ssh-username-goes-here" # Must be the username of your google account example "username@gmail.com" omit "@gmail.com"
tailscale_key = "tailscale-key-goes-here"
ssh-pub-key = "gcp-ssh-key.pub" # Do not change this variable
```


## File structure should be as stated below in your project/folder

![image](https://user-images.githubusercontent.com/48220443/205532426-9a328836-e6c0-4f35-a86b-0e4b83b9d455.png)
