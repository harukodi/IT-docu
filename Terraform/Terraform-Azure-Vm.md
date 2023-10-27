## This terraform file will create a vm in Azure Cloud with entra id for ssh auth

#### main.tf
```HCL
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = "3.77.0"
    }
  }
}

variable "vm_count" {}

variable "ssh_key_location" {}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "dev-test-resource-group" {
  name     = "DEV-TEST-TERRAFORM"
  location = "Norway East"
}

resource "azurerm_virtual_network" "dev-test-virtual-network" {
  name                = "dev-test-virtual-network"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.dev-test-resource-group.location
  resource_group_name = azurerm_resource_group.dev-test-resource-group.name
}

resource "azurerm_subnet" "dev-test-subnet" {
  name                 = "internal"
  resource_group_name  = azurerm_resource_group.dev-test-resource-group.name
  virtual_network_name = azurerm_virtual_network.dev-test-virtual-network.name
  address_prefixes     = ["10.0.2.0/24"]
}

resource "azurerm_network_security_group" "dev-test-nsg" {
  name                = "dev-test-nsg"
  location            = azurerm_resource_group.dev-test-resource-group.location
  resource_group_name = azurerm_resource_group.dev-test-resource-group.name

  security_rule {
    name                       = "nsg-allow-http"
    priority                   = 100
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "*"
    source_port_range          = "*"
    destination_port_range     = "80"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

  security_rule {
    name                       = "nsg-allow-ssh"
    priority                   = 101
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "*"
    source_port_range          = "*"
    destination_port_range     = "22"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

  security_rule {
    name                       = "nsg-default-allow-all-outbound"
    priority                   = 102
    direction                  = "Outbound"
    access                     = "Allow"
    protocol                   = "*"
    source_port_range          = "*"
    destination_port_range     = "*"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

  security_rule {
    name                       = "nsg-default-deny-all-inbound"
    priority                   = 103
    direction                  = "Inbound"
    access                     = "Deny"
    protocol                   = "*"
    source_port_range          = "*"
    destination_port_range     = "*"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

}

resource "azurerm_subnet_network_security_group_association" "dev-test-nsg-association" {
  subnet_id                 = azurerm_subnet.dev-test-subnet.id
  network_security_group_id = azurerm_network_security_group.dev-test-nsg.id
}

resource "azurerm_public_ip" "vm_public_ip" {
  count = var.vm_count
  name                = "public-vm-ip-${count.index}"
  resource_group_name = azurerm_resource_group.dev-test-resource-group.name
  location            = azurerm_resource_group.dev-test-resource-group.location
  allocation_method   = "Static"
}

resource "azurerm_network_interface" "dev-test-network-interface" {
  count = var.vm_count
  name                = "dev-test-nic-${count.index}"
  location            = azurerm_resource_group.dev-test-resource-group.location
  resource_group_name = azurerm_resource_group.dev-test-resource-group.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.dev-test-subnet.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id = azurerm_public_ip.vm_public_ip[count.index].id
  }
}

resource "azurerm_linux_virtual_machine" "dev-test-linux-virtual-machine" {
  count = var.vm_count
  name                = "azure-linux-vm-machine${count.index}"
  resource_group_name = azurerm_resource_group.dev-test-resource-group.name
  location            = azurerm_resource_group.dev-test-resource-group.location
  size                = "Standard_F2"
  admin_username      = "adminuser"
  network_interface_ids = [
    azurerm_network_interface.dev-test-network-interface[count.index].id,
  ]

  admin_ssh_key {
    username   = "adminuser"
    public_key = file("./key.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-focal"
    sku       = "20_04-lts"
    version   = "latest"
  }

  identity {
    type  = "SystemAssigned"
  }

}

resource "azurerm_virtual_machine_extension" "dev-test-linux-virtual-machine-extension" {
  name                 = "init-script"
  count = var.vm_count
  virtual_machine_id   = azurerm_linux_virtual_machine.dev-test-linux-virtual-machine[count.index].id
  publisher            = "Microsoft.Azure.Extensions"
  type                 = "CustomScript"
  type_handler_version = "2.0"

  settings = <<SETTINGS
 {
  "commandToExecute": "apt update -y; apt upgrade -y"
 }
SETTINGS
}

resource "azurerm_virtual_machine_extension" "dev-test-linux-virtual-machine-extension-ssh-entra-id" {
  name                 = "AADLoginForLinux"
  count = var.vm_count
  virtual_machine_id   = azurerm_linux_virtual_machine.dev-test-linux-virtual-machine[count.index].id
  publisher            = "Microsoft.Azure.ActiveDirectory"
  type                 = "AADSSHLoginForLinux"
  auto_upgrade_minor_version = true
  type_handler_version = "1.0"

depends_on = [ azurerm_linux_virtual_machine.dev-test-linux-virtual-machine ]

}
```

#### terraform.tfvars
```HCL
vm_count = 2
ssh_key_location = "./key.pub"
```


## This bash script creates the vm and ssh keys
**NOTE**: You can not use the terraform apply command becuase the ssh keys need to be generated before. Use the bash script to setup the keys and the vm. After that you can use terraform apply if you update resources in your main.tf file.

#### azure-vm-create.sh
```bash
#!/bin/bash

ssh-keygen -b 2048 -t rsa -f key -N ""
terraform apply -auto-approve
```


#
## Your file structure should look like this
![[Pasted image 20231027163429.png]]