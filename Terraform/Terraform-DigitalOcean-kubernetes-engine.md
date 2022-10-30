## main.tf


```HCL
terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "~> 2.0"
    }
  }
}


variable "do_token" {}
variable "cluster_name" {}
variable "cluster_size" {}
variable "number_of_nodes" {}
variable "cluster_region" {}

provider "digitalocean" {
  token = var.do_token
}


data "digitalocean_kubernetes_versions" "kubernetes_version" {}

output "k8s-versions" {
  value = data.digitalocean_kubernetes_versions.kubernetes_version.valid_versions
}


resource "digitalocean_tag" "demo-kubernetes-cluster-tag" {
  name = var.cluster_name
}


resource "digitalocean_kubernetes_cluster" "demo-kubernetes-cluster" {
  name   = "demo-kubernetes-cluster"
  region = var.cluster_region
  version = data.digitalocean_kubernetes_versions.kubernetes_version.latest_version

  node_pool {
    name       = var.cluster_name
    size       = var.cluster_size
    node_count = var.number_of_nodes
    tags = [digitalocean_tag.demo-kubernetes-cluster-tag.id]

    taint {
      key    = "workloadKind"
      value  = "database"
      effect = "NoSchedule"
    }
  }
}

resource "digitalocean_firewall" "demo-kubernetes-cluster-firewall" {
  name = "demo-kubernetes-cluster-firewall"

  tags = [digitalocean_tag.demo-kubernetes-cluster-tag.id]

  inbound_rule {
    protocol         = "tcp"
    port_range       = "1-65535"
    source_addresses = ["10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16"]
  }

  inbound_rule {
    protocol         = "udp"
    port_range       = "1-65535"
    source_addresses = ["10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16"]
  }

  inbound_rule {
    protocol         = "icmp"
    port_range       = "1-65535"
    source_addresses = ["10.0.0.0/8", "172.16.0.0/12", "192.168.0.0/16"]
  }

  outbound_rule {
    protocol              = "tcp"
    port_range            = "1-65535"
    destination_addresses = ["0.0.0.0/0"]
  }

  outbound_rule {
    protocol              = "udp"
    port_range            = "1-65535"
    destination_addresses = ["0.0.0.0/0"]
  }

  outbound_rule {
    protocol              = "icmp"
    port_range            = "1-65535"
    destination_addresses = ["0.0.0.0/0"]
  }

}
```


## terraform.tfvars
```
do_token = "digital-ocean-api-token-here"
cluster_name = "demo-kubernetes-cluster"
cluster_size = "s-1vcpu-2gb"
number_of_nodes = "2"
cluster_region = "fra1"
```