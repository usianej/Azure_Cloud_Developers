# Core Azure concepts

- Introduction to Azure fundamentals
- Azure fundamental concepts
- Core Azure Architectural components
- Creating an Azure Account
_______________________________________________________________________________________________

## Introduction to Azure fundamentals

- Azure compute services
- Azure storage services.
- Azure analytics
- Azure database

## Azure Compute Services

### VM's

Virtual machines (IaaS) are the building blocks of many services in Azure

Often easiest to relate to as closely aligns with a VM on premises.
e.g Using virtual box to set-up a virtual machine (show vmware virtual box)

Pure IaaS with **TOTAL** access to everything inside the VM OS and above

This means you are responsible for everything OS and above.

--------

Often different workloads have different resource type requirements

e.g Like t-shirts there are different sizes like s-small, L-large, XL-xtralarge, etc.

Different VM series have different focus in terms of resource ratios(compute memory anad storage)... what is compute, memory and storage.

> https://docs.microsoft.com/en-us/azure/virtual-machines/sizes

### VM Building Blocks

In ARM there is more than just the VM

- VM [spot] - allows you to get VMs for cheaper.
- NIC[s] (connected to a subnet)
- OS disk (managed disk, could be ephemeral in cache/temp space)
- Temp disk (normally, some do not have)
- Data disks (cache options)
- Extensions
- [Public Infiniband , GPU, NVMe
- Availability Set, Availability Zone, Proximity Placement Groups
- Managed Identity

### Supported OS

- Windows
- Linux
- Large number of images in Azure marketplace
- Create your own images and can make easily available through shared image gallery
- Install your applications and other agents like sccm as required.

### Maintenance Considerations

> These maintainance is for the azure fabric. 

- VMs can encounter various types of issues and maintenance
- Planned maintenance (VM no reboot vs reboot)
- Unplanned hardware maintenance
- Unexpected downtime
- Remember to use Availability Sets/Availability Zones to reduce impact (FDs and UDs)
- Multiple regions in case of region outage

### Host Optons 

- VMs are normally placed on a host with resources from other tenants
- There are options where the host is dedicated
- Isolated VMs
- Dedicated host
- Azure Stack Hub, Edge and HCI (on-premises)
- Bare-metal such as SAP HANA solutions

## VM Scale Sets (VMSS)

- Often a workload requires multiple instances
- Capacity may also vary over time
- VMSS enables a template and configuration to be defined along with scale parameters
- Optionally, automatically part of a load balancer backend set

Note:

    • Fault domains and zone balancing
    • Overprovisioning (spin-up extras by default)
    • Auto image update (Azure Marketplace and shared image galery). Image auto upgrade replaces not more than 20% of the old version with a new version. 
    • Instance repair (auto-healing)
    • Scale-In Policy (how you wnat it to delete)
    • Termination notification (gives you some head's up before termination)
    • Instance protection (protection policy)

## NETWORKING

### VNET BASICS

- A virtual network consists of one or more IP ranges
- Typically from RFC 1918 but not exclusively
- A virtual network exists: 

-----

    - Within a specific subscription
    - Within a specific region
    - It cannot span subscriptions nor regions

- The address space is broken up into subnets with the smallest subnet possible being a /29 which will give 3 usable IP addresses
- Subnet are regional and span Availability Zones

### Reserves IP Addresses

For example, an ip range of 192.168.1.0/24

- 192.168.1.0: Network addresses
- 192.168.1.1: Reserved by Azure fir the default gateway
- 192.168.1.2, 192.168.1.4: Reserved by Azure to map the DNS IPs to the VNet space
- 192.168.1.255: Network Broadcast address.

> A specific virtual machine cannot span VNets, but the traffic can flow between VNets if configured.

### VM NIC

```markdown
IP always comes via fabric (OS using DHCP)
```
- IP can be reserved in ARM
- VMs can be configured with multiple NICs
- Each NIC can be in different virtual subnet in same virtual network or different subnets
- Multiple IP configurations per NIC
- IP configuration has private IP and optional public IP
