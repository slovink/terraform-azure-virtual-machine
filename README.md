<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AZURE VIRTUAL MACHINE
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create VIRTUAL MACHINE resource on AZURE.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v1.1.7-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-APACHE-blue.svg" alt="Licence">
</a>






## Prerequisites

This module has a few dependencies:

- [Terraform 1.x.x](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)







## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/slovink/terraform-azure-virtual-machine/releases).


### Simple Example
Here is an example of how you can use this module in your inventory structure:
  ```hcl

module "virtual-machine" {
  source = "../../"

  ## Tags
  name        = "app"
  environment = "test"
  label_order = ["environment", "name"]

  ## Common
  is_vm_linux                     = true
  enabled                         = true
  machine_count                   = 1
  resource_group_name             = module.resource_group.resource_group_name
  location                        = module.resource_group.resource_group_location
  disable_password_authentication = true

  ## Network Interface
  subnet_id                     = module.subnet.default_subnet_id
  private_ip_address_version    = "IPv4"
  private_ip_address_allocation = "Static"
  primary                       = true
  private_ip_addresses          = ["10.0.1.4"]
  #nsg
  network_interface_sg_enabled = true
  network_security_group_id    = module.security_group.id

  ## Availability Set
  availability_set_enabled     = true
  platform_update_domain_count = 7
  platform_fault_domain_count  = 3

  ## Public IP
  public_ip_enabled = true
  sku               = "Basic"
  allocation_method = "Static"
  ip_version        = "IPv4"


  ## Virtual Machine
  linux_enabled      = true
  vm_size            = "Standard_B1s"
  public_key         = "ssh-rsa AAAAB3NzaC1yc2EoL9X+2+4Xb dev" # Enter valid public key here
  username           = "ubuntu"
  os_profile_enabled = true
  admin_username     = "ubuntu"
  # admin_password                  = "P@ssw0rd!123!" # It is compulsory when disable_password_authentication = false
  create_option                   = "FromImage"
  caching                         = "ReadWrite"
  disk_size_gb                    = 30
  os_type                         = "Linux"
  managed_disk_type               = "Standard_LRS"
  storage_image_reference_enabled = true
  image_publisher                 = "Canonical"
  image_offer                     = "0001-com-ubuntu-server-focal"
  image_sku                       = "20_04-lts"
  image_version                   = "latest"
}

  ```



## Feedback
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/slovink/terraform-azure-virtual-machine/issues), or feel free to drop us an email at [devops@slovink.com](mailto:devops@slovink.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/slovink/terraform-azure-virtual-machine!
