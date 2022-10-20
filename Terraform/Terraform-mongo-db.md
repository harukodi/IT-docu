# Initiate a mongo-db cluster on atlas with terraform


## Create a file .tf file example main.tf
```HCL
terraform {
  required_providers {
    mongodbatlas = {
      source = "mongodb/mongodbatlas"
      version = "1.4.4"
    }
  }
}

#Variable for the public key at mongodb
variable "pub_key" {
  type = string
  default = "Public-key-goes-here"
}

#Variable for the private key at mongodb
variable "priv_key" {
  type = string
  default = "Private-key-goes-here"
}

#Username for the database user
variable "username" {
  type = string
  default = "database-username-goes-here"
}

#Password for the database user
variable "password" {
  type = string
  default = "database-user-password-goes-here"
}

#Unique project id for the database cluster
variable "project_id" {
  type = string
  default = "project-id-goes-here"
}

#Org-ID from mongodb's website
variable "org_id" {
  default = "org-id-goes-here"
}

#Fetch public ip for the ip whitelisting at mongodb
data "http" "public_ip" {
  url = "https://ifconfig.me"
}

provider "mongodbatlas" {
  public_key = var.pub_key #required
  private_key  = var.priv_key #required
}

resource "mongodbatlas_project" "demo-db" {
  name   = var.project_id
  org_id = var.org_id
}

data "mongodbatlas_project" "demo-db" {
  project_id = "${mongodbatlas_project.demo-db.id}"
}

resource "mongodbatlas_cluster" "cluster-test" {
  project_id              = var.project_id
  name                    = "demo-db"

  # Provider Settings "block"
  provider_name = "TENANT"
  backing_provider_name = "AWS"
  provider_region_name = "EU_NORTH_1"
  provider_instance_size_name = "M0"
}

resource "mongodbatlas_database_user" "demo-db" {
  username           = var.username
  password           = var.password
  project_id         = var.project_id
  auth_database_name = "admin"

  roles {
    role_name     = "readWrite"
    database_name = "admin"
  }
}

resource "mongodbatlas_project_ip_access_list" "demo-db" {
  project_id = var.project_id
  ip_address = data.http.public_ip.response_body
  comment    = "Allow access to db with ip whitelisting"
}
```


## Run the terraform file
```bash
terraform apply -auto-approve
```


