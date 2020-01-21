# Deploy and manage virtual machines (VMs)

## Create and configure a VM for Windows and Linux

* configure high availability
* configure monitoring, networking, storage, and virtual machine size
* [deploy and configure scale sets](https://blogs.technet.microsoft.com/tangent_thoughts/2016/08/25/got-vm-scale-sets-aka-msazurevmss/)

## Automate deployment of VMs

* modify Azure Resource Manager (ARM) template
  * JSON files with a predefined syntax and schema
    * parameters
    * variables
    * resources
    * outputs
  * Azure and Azure Stack compatible
  * Deployment from:
    * [Azure Portal / Add Resource / Template Deployment](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/quickstart-create-templates-use-the-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
    * [PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-vm?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
    * [Azure CLI](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-cli)
  * GitHub -> [Azure QuickStart Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)
  * Visual Studio Code
  * Visual Studio 2015 adn up (all editions)
* configure location of new VMs
* configure VHD template
* deploy from template
* save a deployment as an ARM template
  * If you want to export a template with all of the resources in a resource group, you can export the ARM template from the resource group level.

```Powershell
Export-AzResourceGroup
      -ResourceGroupName <String>
      [-Path <String>]
      [-IncludeParameterDefaultValue]
      [-IncludeComments]
      [-Force]
      [-ApiVersion <String>]
      [-Pre]
      [-DefaultProfile <IAzureContextContainer>]
      [-InformationAction <ActionPreference>]
      [-InformationVariable <String>]
      [-WhatIf]
      [-Confirm]
      [<CommonParameters>]
```

  * The parameters file exported contains the parameters that were used during that deployment.
  * The template contains needed scripts for deploying the template. When you download an ARM template from a deployment or a resource group, the downloaded package contains 7 files:
    * the ARM template,
    * the parameters file,
    * an Azure CLI script,
    * a Powershell script,
    * a Ruby script,
    * and a .NET class for deploying the template.
* deploy Windows and Linux VMs

## Manage Azure VM

* add data discs
* add network interfaces
* [automate configuration management by using PowerShell Desired State Configuration (DSC) and VM Agent by using custom script extensions](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-template)
* manage VM sizes
* move VMs from one resource group to another
* redeploy VMs
* soft delete for Azure VMs

| VM Types | Purpose |
|----|----|
| A - Basic | Basic version of the A series for testing and development |
| A - Standard | General-purpose VMs |
| B - Burstable | Can burst to the full capactiy of the CPU when needed |
| D - General Purpose | Built for enterprise applications. DS instances offer premium storage. |
| E - Memory Optimized | High memory-to-CPU core ratio. ES instances offer premium storage. |
| F - CPU Optimized | High CPU-to-memory ratio. FS instances offer premium storage. |
| G - Godzilla | Very large instances ideal for large databases and big data use cases. |
| H - High-performance computer | High performance computer instances aimed at very high-end computational needs such as molecular modeling and other scientific applications. |
| L - Storage optimized | Storage optimized instances which offer a higher disk throughput and IO. |
| M - Large Memory | Another large-scale memory option that allows for up to 3.5 TB of RAM. |
| N - GPU enabled | GPU-enabled instances |
| SAP HANA on Azure Certified Insances | Specialized instances purposely built and certified for running SAP HANA. |

| VM Specializations | Description |
|----|----|
| S | Premium storage options available (ex. DSv2) |
| M | Larger memory configuration of instance type (ex. A2m_v2) |
| R | Supports remote direct memory access - RDMA (ex. H16mr) |

| Azure Computer Units (ACUs) |
|----|
| Way to compare CPU performance between different types/sizes of VM |
| Microsoft created performance benchmark |
| A VM with an ACU of 200 has twice the performance of a VM with an ACU of 100 |

**Disk Caching**
* Method for improving performance of VHDs
* Utilizes local RAM and SSH drives on underlying VM host
* Available on both standard and premium disks
* Read-Only Caching
  - Improve latency and potentially gain higher IOPS per disk
* Read-Write Caching
  - Ensure you have a proper way to write data from cache to persistent disks

**Default and Allowed Settings**

| Disk Type | Default Cache Setting | Allowed Settings |
|----|----|----|
| OS Disk | Read-Write | Read-Only or Read-Write |
| Data Disk | None | None, Read-Only, or Read-Write |

### Connecting via WinRM

| WinRMHttps | WinRMHttp |
|----|----|
| Connectivity over SSL | No SSL required |
| Uses port 5986 | Uses port 5985 |
| Recommended to use this option if you are going over the Internet, as it is much more secure | Ideal for connections from the same private network which is already secure |

## Manage VM backups

* configure VM backup
* define backup policies
* implement backup policies
* perform VM restore
* [Azure Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)

| Supported | Details |
|----|----|
| Replication scenarios | • Replicate Azure VMs from one Azure region to another. <br/>• Replicate on-premises VMware VMs, Hyper-V VMs, physical servers (Windows and Linux), Azure Stack VMs to Azure.<br/>• Replicate AWS Windows instances to Azure.<br/>• Replicate on-premises VMware VMs, Hyper-V VMs managed by System Center VMM, and physical servers to a secondary site.|
| Regions | Review supported regions for Site Recovery. |
| Replicated machines | Review the replication requirements for Azure VM replication, on-premises VMware VMs and physical servers, and on-premises Hyper-V VMs. |
| Workloads | You can replicate any workload running on a machine that's supported for replication. In addition, the Site Recovery team have performed app-specific testing for a number of apps. |
