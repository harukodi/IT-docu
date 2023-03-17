
## main.tf
```HCL
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.43.1"
    }
    kubernetes = {
      source = "hashicorp/kubernetes"
      version = "2.18.1"
    }
  }
}

variable "project_id" {}
variable "region" {}
variable "zone" {}
variable "gcp_key_file" {}
variable "node_count" {}
variable "cluster_name" {}
variable "persistent_disk_name" {}

provider "google" {
  project     = var.project_id
  region      = var.region
  credentials = file(var.gcp_key_file)
}

resource "google_compute_disk" "compute_disk_default" {
  name  = var.persistent_disk_name
  type  = "pd-ssd"
  zone  = var.zone
  size  = 10
  physical_block_size_bytes = 4096
}

resource "google_container_cluster" "default" {
  name     = var.cluster_name
  location = var.zone
  remove_default_node_pool = true
  initial_node_count       = 1
}

resource "google_container_node_pool" "default_cluster" {
  name       = var.cluster_name
  location   = var.zone
  cluster    = google_container_cluster.default.name
  node_count = var.node_count

  node_config {
    preemptible  = true
    machine_type = "e2-medium"
  }
}

# Retrieve an access token as the Terraform runner
data "google_client_config" "provider" {}

data "google_container_cluster" "gke_k8s_cluster" {
  name     = var.cluster_name
  location = var.zone
  depends_on = [
    google_container_node_pool.default_cluster,
    google_container_cluster.default
  ]
}

provider "kubernetes" {
  host  = "https://${data.google_container_cluster.gke_k8s_cluster.endpoint}"
  token = data.google_client_config.provider.access_token
  cluster_ca_certificate = base64decode(
    data.google_container_cluster.gke_k8s_cluster.master_auth[0].cluster_ca_certificate,
  )
}

resource "kubernetes_persistent_volume" "pd_default" {
  metadata {
    name = var.persistent_disk_name
  }
  spec {
    capacity = {
      storage = "10Gi"
    }
    access_modes = ["ReadWriteMany"]
    persistent_volume_source {
      gce_persistent_disk {
        fs_type = "ext4"
        pd_name = var.persistent_disk_name
      }
    }
  }
  depends_on = [
    google_compute_disk.compute_disk_default,
    google_container_node_pool.default_cluster,
    google_container_cluster.default
  ]
}
```
## terraform.tfvars
```HCL
project_id = "project-id-goes-here"
region = "region-goes-here"
zone = "zone-goes-here"
gcp_key_file = "gcp-key.json"
cluster_name = "cluster-name-goes-here"
node_count = 2
persistent_disk_name = "persistent-disk-name-goes-here"
```