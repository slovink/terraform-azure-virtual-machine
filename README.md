<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AZURE VIRTUAL MACHINE
</h1>

<p align="center" style="font-size: 1.2rem;">
    Terraform module to create VIRTUAL MACHINE resource on AZURE.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v1.7.4-green" alt="Terraform">
</a>
<a href="https://github.com/slovink/terraform-azure-virtual-machine/blob/dev/LICENSE">
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
  source      = "https://github.com/slovink/terraform-azure-virtual-machine.git?ref=1.0.0"
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

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/slovink/terraform-azure-virtual-machine/blob/dev/LICENSE) file for details.

## Examples
For detailed examples on how to use this module, please refer to the [Examples](https://github.com/slovink/terraform-azure-virtual-machine/tree/dev/_example) directory within this repository.



## Feedback
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/slovink/terraform-azure-virtual-machine/issues), or feel free to drop us an email at [contact@slovink.com](contact@slovink.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/slovink/terraform-azure-virtual-machine!

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >=1.7.4 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | >=3.87.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | >=3.87.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | https://github.com/slovink/terraform-azure-labels.git | 1.0.0 |

## Resources

| Name | Type |
|------|------|
| [azurerm_availability_set.default](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/availability_set) | resource |
| [azurerm_network_interface.default](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_interface) | resource |
| [azurerm_network_interface_security_group_association.default](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_interface_security_group_association) | resource |
| [azurerm_public_ip.default](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/public_ip) | resource |
| [azurerm_virtual_machine.default](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/virtual_machine) | resource |
| [azurerm_virtual_machine.win_vm](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/virtual_machine) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_addtional_capabilities_enabled"></a> [addtional\_capabilities\_enabled](#input\_addtional\_capabilities\_enabled) | Whether additional capabilities block is enabled. | `bool` | `false` | no |
| <a name="input_admin_password"></a> [admin\_password](#input\_admin\_password) | The password associated with the local administrator account. | `string` | `"password123"` | no |
| <a name="input_admin_username"></a> [admin\_username](#input\_admin\_username) | Specifies the name of the local administrator account. | `string` | `"krishna"` | no |
| <a name="input_allocation_method"></a> [allocation\_method](#input\_allocation\_method) | Defines the allocation method for this IP address. Possible values are Static or Dynamic. | `string` | `""` | no |
| <a name="input_availability_set_enabled"></a> [availability\_set\_enabled](#input\_availability\_set\_enabled) | Whether availability set is enabled. | `bool` | `false` | no |
| <a name="input_blob_endpoint"></a> [blob\_endpoint](#input\_blob\_endpoint) | The Storage Account's Blob Endpoint which should hold the virtual machine's diagnostic files | `string` | `""` | no |
| <a name="input_boot_diagnostics_enabled"></a> [boot\_diagnostics\_enabled](#input\_boot\_diagnostics\_enabled) | Whether boot diagnostics block is enabled. | `bool` | `false` | no |
| <a name="input_caching"></a> [caching](#input\_caching) | Specifies the caching requirements for the OS Disk. Possible values include None, ReadOnly and ReadWrite. | `string` | `""` | no |
| <a name="input_certificate_store"></a> [certificate\_store](#input\_certificate\_store) | Specifies the certificate store on the Virtual Machine where the certificate should be added to, such as My. | `string` | `""` | no |
| <a name="input_certificate_url"></a> [certificate\_url](#input\_certificate\_url) | The ID of the Key Vault Secret which contains the encrypted Certificate which should be installed on the Virtual Machine. This certificate must also be specified in the vault\_certificates block within the os\_profile\_secrets block. | `string` | `""` | no |
| <a name="input_computer_name"></a> [computer\_name](#input\_computer\_name) | Name of the Windows Computer Name. | `string` | `""` | no |
| <a name="input_create"></a> [create](#input\_create) | Used when creating the Resource Group. | `string` | `"60m"` | no |
| <a name="input_create_option"></a> [create\_option](#input\_create\_option) | Specifies how the OS Disk should be created. Possible values are Attach (managed disks only) and FromImage. | `string` | `""` | no |
| <a name="input_custom_image_id"></a> [custom\_image\_id](#input\_custom\_image\_id) | Specifies the ID of the Custom Image which the Virtual Machine should be created from. | `string` | `""` | no |
| <a name="input_ddos_protection_mode"></a> [ddos\_protection\_mode](#input\_ddos\_protection\_mode) | The DDoS protection mode of the public IP | `string` | `"VirtualNetworkInherited"` | no |
| <a name="input_delete"></a> [delete](#input\_delete) | Used when deleting the Resource Group. | `string` | `"60m"` | no |
| <a name="input_delete_data_disks_on_termination"></a> [delete\_data\_disks\_on\_termination](#input\_delete\_data\_disks\_on\_termination) | Should the Data Disks (either the Managed Disks / VHD Blobs) be deleted when the Virtual Machine is destroyed? Defaults to false. | `bool` | `true` | no |
| <a name="input_delete_os_disk_on_termination"></a> [delete\_os\_disk\_on\_termination](#input\_delete\_os\_disk\_on\_termination) | Should the OS Disk (either the Managed Disk / VHD Blob) be deleted when the Virtual Machine is destroyed? Defaults to false. | `bool` | `true` | no |
| <a name="input_disable_password_authentication"></a> [disable\_password\_authentication](#input\_disable\_password\_authentication) | Specifies whether password authentication should be disabled. | `bool` | `false` | no |
| <a name="input_disk_size_gb"></a> [disk\_size\_gb](#input\_disk\_size\_gb) | Specifies the size of the OS Disk in gigabytes. | `number` | `8` | no |
| <a name="input_dns_servers"></a> [dns\_servers](#input\_dns\_servers) | List of IP addresses of DNS servers. | `list(string)` | `[]` | no |
| <a name="input_domain_name_label"></a> [domain\_name\_label](#input\_domain\_name\_label) | Label for the Domain Name. Will be used to make up the FQDN. If a domain name label is specified, an A DNS record is created for the public IP in the Microsoft Azure DNS system. | `string` | `null` | no |
| <a name="input_enable_accelerated_networking"></a> [enable\_accelerated\_networking](#input\_enable\_accelerated\_networking) | Should Accelerated Networking be enabled? Defaults to false. | `bool` | `false` | no |
| <a name="input_enable_ip_forwarding"></a> [enable\_ip\_forwarding](#input\_enable\_ip\_forwarding) | Should IP Forwarding be enabled? Defaults to false. | `bool` | `false` | no |
| <a name="input_enable_os_disk_write_accelerator"></a> [enable\_os\_disk\_write\_accelerator](#input\_enable\_os\_disk\_write\_accelerator) | Should Write Accelerator be Enabled for this OS Disk? This requires that the `storage_account_type` is set to `Premium_LRS` and that `caching` is set to `None`. | `string` | `false` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Flag to control the module creation. | `bool` | `false` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| <a name="input_identity_enabled"></a> [identity\_enabled](#input\_identity\_enabled) | Whether identity block is enabled. | `bool` | `false` | no |
| <a name="input_identity_ids"></a> [identity\_ids](#input\_identity\_ids) | Specifies a list of user managed identity ids to be assigned to the VM. | `list(any)` | `[]` | no |
| <a name="input_idle_timeout_in_minutes"></a> [idle\_timeout\_in\_minutes](#input\_idle\_timeout\_in\_minutes) | Specifies the timeout for the TCP idle connection. The value can be set between 4 and 60 minutes. | `number` | `10` | no |
| <a name="input_image_offer"></a> [image\_offer](#input\_image\_offer) | Specifies the offer of the image used to create the virtual machine. | `string` | `""` | no |
| <a name="input_image_publisher"></a> [image\_publisher](#input\_image\_publisher) | Specifies the publisher of the image used to create the virtual machine. | `string` | `""` | no |
| <a name="input_image_sku"></a> [image\_sku](#input\_image\_sku) | Specifies the SKU of the image used to create the virtual machine. | `string` | `""` | no |
| <a name="input_image_uri"></a> [image\_uri](#input\_image\_uri) | Specifies the Image URI in the format publisherName:offer:skus:version. This field can also specify the VHD uri of a custom VM image to clone. When cloning a Custom (Unmanaged) Disk Image the os\_type field must be set. | `string` | `""` | no |
| <a name="input_image_version"></a> [image\_version](#input\_image\_version) | Specifies the version of the image used to create the virtual machine. | `string` | `""` | no |
| <a name="input_internal_dns_name_label"></a> [internal\_dns\_name\_label](#input\_internal\_dns\_name\_label) | The (relative) DNS Name used for internal communications between Virtual Machines in the same Virtual Network. | `string` | `null` | no |
| <a name="input_ip_version"></a> [ip\_version](#input\_ip\_version) | The IP Version to use, IPv6 or IPv4. | `string` | `""` | no |
| <a name="input_is_vm_linux"></a> [is\_vm\_linux](#input\_is\_vm\_linux) | Create Linux Virtual Machine. | `bool` | `false` | no |
| <a name="input_is_vm_windows"></a> [is\_vm\_windows](#input\_is\_vm\_windows) | Create Windows Virtual Machine. | `bool` | `false` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. `name`,`application`. | `list(any)` | `[]` | no |
| <a name="input_location"></a> [location](#input\_location) | Location where resource should be created. | `string` | `""` | no |
| <a name="input_lun"></a> [lun](#input\_lun) | Specifies the logical unit number of the data disk. This needs to be unique within all the Data Disks on the Virtual Machine. | `number` | `0` | no |
| <a name="input_machine_count"></a> [machine\_count](#input\_machine\_count) | Number of Virtual Machines to create. | `number` | `0` | no |
| <a name="input_managed"></a> [managed](#input\_managed) | Specifies whether the availability set is managed or not. Possible values are true (to specify aligned) or false (to specify classic). Default is true. | `bool` | `true` | no |
| <a name="input_managed_disk_id"></a> [managed\_disk\_id](#input\_managed\_disk\_id) | Specifies the ID of an existing Managed Disk which should be attached as the OS Disk of this Virtual Machine. If this is set then the create\_option must be set to Attach. | `string` | `""` | no |
| <a name="input_managed_disk_type"></a> [managed\_disk\_type](#input\_managed\_disk\_type) | Specifies the type of Managed Disk which should be created. Possible values are Standard\_LRS, StandardSSD\_LRS or Premium\_LRS. | `string` | `""` | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | slovink | `string` | `"contact@slovink.com"` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| <a name="input_network_interface_sg_enabled"></a> [network\_interface\_sg\_enabled](#input\_network\_interface\_sg\_enabled) | Whether network interface security group is enabled. | `bool` | `false` | no |
| <a name="input_network_security_group_id"></a> [network\_security\_group\_id](#input\_network\_security\_group\_id) | The ID of the Network Security Group which should be attached to the Network Interface. | `string` | `""` | no |
| <a name="input_os_profile_enabled"></a> [os\_profile\_enabled](#input\_os\_profile\_enabled) | Whether os profile block is enabled. | `bool` | `false` | no |
| <a name="input_os_type"></a> [os\_type](#input\_os\_type) | Specifies the Operating System on the OS Disk. Possible values are Linux and Windows. | `string` | `""` | no |
| <a name="input_plan_enabled"></a> [plan\_enabled](#input\_plan\_enabled) | Whether plan block is enabled. | `bool` | `false` | no |
| <a name="input_plan_name"></a> [plan\_name](#input\_plan\_name) | Specifies the name of the image from the marketplace. | `string` | `""` | no |
| <a name="input_plan_product"></a> [plan\_product](#input\_plan\_product) | Specifies the product of the image from the marketplace. | `string` | `""` | no |
| <a name="input_plan_publisher"></a> [plan\_publisher](#input\_plan\_publisher) | Specifies the publisher of the image. | `string` | `""` | no |
| <a name="input_platform_fault_domain_count"></a> [platform\_fault\_domain\_count](#input\_platform\_fault\_domain\_count) | Specifies the number of fault domains that are used. Defaults to 3. | `number` | `3` | no |
| <a name="input_platform_update_domain_count"></a> [platform\_update\_domain\_count](#input\_platform\_update\_domain\_count) | Specifies the number of update domains that are used. Defaults to 5. | `number` | `5` | no |
| <a name="input_primary"></a> [primary](#input\_primary) | Is this the Primary IP Configuration? Must be true for the first ip\_configuration when multiple are specified. Defaults to false. | `bool` | `false` | no |
| <a name="input_private_ip_address_allocation"></a> [private\_ip\_address\_allocation](#input\_private\_ip\_address\_allocation) | The allocation method used for the Private IP Address. Possible values are Dynamic and Static. | `string` | `"Static"` | no |
| <a name="input_private_ip_address_version"></a> [private\_ip\_address\_version](#input\_private\_ip\_address\_version) | The IP Version to use. Possible values are IPv4 or IPv6. Defaults to IPv4. | `string` | `"IPv4"` | no |
| <a name="input_private_ip_addresses"></a> [private\_ip\_addresses](#input\_private\_ip\_addresses) | The Static IP Address which should be used. | `list(any)` | `[]` | no |
| <a name="input_proximity_placement_group_id"></a> [proximity\_placement\_group\_id](#input\_proximity\_placement\_group\_id) | The ID of the Proximity Placement Group to which this Virtual Machine should be assigned. | `string` | `""` | no |
| <a name="input_public_ip_enabled"></a> [public\_ip\_enabled](#input\_public\_ip\_enabled) | Whether public IP is enabled. | `bool` | `false` | no |
| <a name="input_public_ip_prefix_id"></a> [public\_ip\_prefix\_id](#input\_public\_ip\_prefix\_id) | If specified then public IP address allocated will be provided from the public IP prefix resource. | `string` | `null` | no |
| <a name="input_public_key"></a> [public\_key](#input\_public\_key) | Name  (e.g. `ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD3F6tyPEFEzV0LX3X8BsXdMsQ`). | `string` | `""` | no |
| <a name="input_read"></a> [read](#input\_read) | Used when retrieving the Resource Group. | `string` | `"5m"` | no |
| <a name="input_repository"></a> [repository](#input\_repository) | Terraform current module repo | `string` | `""` | no |
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | The name of the resource group in which to create the virtual network. | `string` | `""` | no |
| <a name="input_reverse_fqdn"></a> [reverse\_fqdn](#input\_reverse\_fqdn) | A fully qualified domain name that resolves to this public IP address. If the reverseFqdn is specified, then a PTR DNS record is created pointing from the IP address in the in-addr.arpa domain to the reverse FQDN. | `string` | `""` | no |
| <a name="input_sku"></a> [sku](#input\_sku) | The SKU of the Public IP. Accepted values are Basic and Standard. Defaults to Basic. | `string` | `"Basic"` | no |
| <a name="input_source_image_id"></a> [source\_image\_id](#input\_source\_image\_id) | The ID of an Image which each Virtual Machine should be based on | `string` | `null` | no |
| <a name="input_source_vault_id"></a> [source\_vault\_id](#input\_source\_vault\_id) | Specifies the ID of the Key Vault to use. | `string` | `""` | no |
| <a name="input_storage_data_disk_enabled"></a> [storage\_data\_disk\_enabled](#input\_storage\_data\_disk\_enabled) | Whether storage data disk is enabled. | `bool` | `false` | no |
| <a name="input_storage_image_reference_enabled"></a> [storage\_image\_reference\_enabled](#input\_storage\_image\_reference\_enabled) | Whether storage image reference is enabled. | `bool` | `false` | no |
| <a name="input_subnet_id"></a> [subnet\_id](#input\_subnet\_id) | The ID of the Subnet where this Network Interface should be located in. | `list(any)` | `[]` | no |
| <a name="input_ultra_ssd_enabled"></a> [ultra\_ssd\_enabled](#input\_ultra\_ssd\_enabled) | Should Ultra SSD disk be enabled for this Virtual Machine?. | `bool` | `false` | no |
| <a name="input_update"></a> [update](#input\_update) | Used when updating the Resource Group. | `string` | `"60m"` | no |
| <a name="input_username"></a> [username](#input\_username) | The linux user name. | `string` | `"krishna"` | no |
| <a name="input_vault_enabled"></a> [vault\_enabled](#input\_vault\_enabled) | Whether key vault is enabled. | `bool` | `false` | no |
| <a name="input_vhd_uri"></a> [vhd\_uri](#input\_vhd\_uri) | Specifies the URI of the VHD file backing this Unmanaged OS Disk. Changing this forces a new resource to be created. | `string` | `null` | no |
| <a name="input_vm_size"></a> [vm\_size](#input\_vm\_size) | Specifies the size of the Virtual Machine. | `string` | `""` | no |
| <a name="input_vm_type"></a> [vm\_type](#input\_vm\_type) | The Managed Service Identity Type of this Virtual Machine. Possible values are SystemAssigned and UserAssigned. | `string` | `""` | no |
| <a name="input_windows_enabled"></a> [windows\_enabled](#input\_windows\_enabled) | Whether windows block is enabled. | `bool` | `false` | no |
| <a name="input_write_accelerator_enabled"></a> [write\_accelerator\_enabled](#input\_write\_accelerator\_enabled) | Specifies if Write Accelerator is enabled on the disk. This can only be enabled on Premium\_LRS managed disks with no caching and M-Series VMs. Defaults to false. | `bool` | `false` | no |
| <a name="input_zones"></a> [zones](#input\_zones) | A collection containing the availability zone to allocate the Public IP in. | `list(any)` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_availability_set_id"></a> [availability\_set\_id](#output\_availability\_set\_id) | The ID of the Availability Set. |
| <a name="output_network_interface_id"></a> [network\_interface\_id](#output\_network\_interface\_id) | The ID of the Network Interface. |
| <a name="output_network_interface_private_ip_addresses"></a> [network\_interface\_private\_ip\_addresses](#output\_network\_interface\_private\_ip\_addresses) | The private IP addresses of the network interface. |
| <a name="output_network_interface_sg_association_id"></a> [network\_interface\_sg\_association\_id](#output\_network\_interface\_sg\_association\_id) | The (Terraform specific) ID of the Association between the Network Interface and the Network Interface. |
| <a name="output_public_ip_address"></a> [public\_ip\_address](#output\_public\_ip\_address) | The IP address value that was allocated. |
| <a name="output_public_ip_id"></a> [public\_ip\_id](#output\_public\_ip\_id) | The Public IP ID. |
| <a name="output_tags"></a> [tags](#output\_tags) | The tags associated to resources. |
| <a name="output_virtual_machine_id"></a> [virtual\_machine\_id](#output\_virtual\_machine\_id) | The ID of the Virtual Machine. |
<!-- END_TF_DOCS -->
