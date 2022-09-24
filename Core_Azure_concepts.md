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

### Host Options 

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
- It is an emulation of physical networking infrastructure
- Designed for isoation, segmentatiom, communicatiom, filtering, routing between resurcs=es
- It is used to connect cloud and on-premise resources
- It is used to protect and monitor services 
- It helps with application delivery 
- Vnet Peering or VPN Gateway allow cross VNet communication
- A virtual network exists: 

-----

    - Within a specific subscription
    - Within a specific region
    - It cannot span subscriptions nor regions

### Subnets

- The address space is broken up into subnets with the smallest subnet possible being a /29 which will give 3 usable IP addresses
- Subnet are regional and span Availability Zones
- Allow customers to group IP addresses in a more efficient manner and to group related resources together to allow security rules and filtering (using NSG and ASG)

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

### LAB

> https://linuxhint.com/install_apache_web_server_ubuntu/


### Supported types of IP traffic

Standard IP based protocols supported including:

- TCP
- UDP
- ICMP
- Multicast, broadcast, IP in IP encapsulated packets and Generic Routing Encapsulation (GRE) blocked
> This is not a seperate VLAN. These are **SDN** there is no layer 2 going on here.
- You cannot ping the Azure gateway or use tools such
as tracert.
- Traditional Layer 2 VLANs are not supported

### IPv6?

- Virtual Networks are dual stack enabling IPv4 and IPv6 address ranges assigned
- IPv6 support in NSG, UDR, LB, peering etc.
- NIC CANNOT be IPv6 only
- Can enable IPv6 for existing resources (may
require reboot)
- No ExpressRoute IPv6 (yet!)
- Public IPs can be IPv4 or IPv6

### External Access

- There is no special “DMZ” subnet where resources get a
public IP
- By default Azure provides outbound SNAT/PAT enabling
resources to access the Internet and receive responses
- To provide services to the Internet either

> Give the IP configuration an instance level public I (not a good idea)

> Place the instances behind an Azure load balancer, gateway or NVA which has a public IP in the front end configuration

> Use a network virtual appliance with a public IP

- Care should be taken to only expose the ports required, e.g. 443


### Load Balancer

- Even traffic distribution
- Supports both inbound and outbound scenarios
- High-availability and scalability scenarios
- Both TCP and UDP traffic 
- External and internal traffic

### Application Gateway

It is a webtraffic load balancer in azure.

- web application firewall
- Redirection
- Session affinity 
- URL Routing 
- SSL termination 

### Content Delivry Network

- Deliver web content to users
- Minimize latency 
- POP (points of presence) LOcations

## AZURE STORAGE

### Data Types

Describe products available for Storage such as Blob Storage, File Storage, Queue Storage, Table Storage, Disk Storage, and Storage Tiers

- Structured - Data that can be represented using tables with very strict schema. Each row must follow defined schema. Some tables have defined relationships between them. Typically used in relational databases.
- Semi-structured - Data that can be represented using tables but without strict defined schema. Rows must only have unique key identifier.
- Unstructured - Any files in any format. Like binary files, application files, images, movies, etc.

### Storage Account

Group of services which include:

    blob storage,
    queue storage,
    table storage, and
    file storage

Used to store:

    files,
    messages, and
    semi-structured data

- Highly scalable (up to petabytes of data)
- Highly durable (99.999999999% - 11 nines, up to 16 nines)
- Cheapest per GB storage

### Blob Storage

- BLOB – binary large object – file
- Designed for storage of files of any kind

Three storage tiers:

    Hot – frequently accessed data
    Cool – infrequently accessed data (lower availability, high durability)
    Archive – rarely (if-ever) accessed data

### Queue Storage

- Storage for small pieces of data (messages)
- Designed for scalable asynchronous processing

> A simple storage with a specific purpose

### Table Storage

Storage for semi-structured data (NoSQL)

    - No need for foreign joins, foreign keys, relationships or strict schema
    - Designed for fast access

Many programming interfaces (API) and SDKs

### File Storage

- Storage for files accessed via shared drive protocols
- Designed to extend on-premise file shares or implement lift-and-shift scenarios

### Disk Storage

- Disk emulation in the cloud
- Persistent storage for Virtual Machines
- Different

    sizes,
    types (SSD, HDD)
    performance tiers
- Disk can be unmanaged or managed

>> Azure Data Lake Storage: Azure storage for BIG DATA

## Azure Database

### Cosmos DB

- Globally distributed NoSQL (semi-structured data) Database service
- Schema-less
- Multiple APIs (SQL, MongoDB, Cassandra, Gremlin, Table Storage)

Designed for:

- Highly responsive (real time) applications with super low latency responses <10ms
- Multi-regional applications

### SQL Database

- Relational database service in the cloud (PaaS) (DBaaS - Database as a Service)
- Structured data service defined using schema and relationships
- Rich Query Capabilities (SQL)
- High-performance, reliable, fully managed and secure database for building - applications

### Azure SQL product family

- Azure SQL Database – Reliable relational database based on SQL Server
- Azure Database for MySQL – Azure SQL version for MySQL database engine
- Azure Database for PostgreSQL – Azure SQL version for PostgreSQL database engine
- Azure SQL Managed Instance – Fully fledged SQL Server managed by cloud provider
- Azure SQL on VM – Fully fledged SQL Server on IaaS
- Azure SQL Data Warehouse (Synapse) – Massively Parallel Processing (MPP) version of SQL Server

Per4mance67895

Per4mance67895

### AZURE_SQL_DB LAB

> After creating database, go to the resorce group and select the server/networking/(change public access to selected networks and add your home IP to the firewall)

Create Table Movies(
    ID int identity(1,1) Primary Key,
    Title Varchar(100) null,
    Genres Varchar(100) null
);


INSERT INTO Movies (Title,Genres)
VALUES
('Toy Story (1995)','Adventure'),
('Iron Man (2020)','Action'),
('Aki na Pawpaw (2021)','Comedy')


SELECT * From [dbo].[Movies]
WHERE Genres LIKE '%Comedy%'

# IOT

create IOT Hub

> go to IOT Hub resource, click on devices,  click on add device 
> select the device and copy the primary connectio string and paste it in the simulator.

https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted

# Setting UP powershell for azure 

$PSVersionTable

> Install-Module az

Get-command *AzAccount* -Module *Az*

>  Connect-AzAccount

### Creat a RG

> New-AzResourceGroup -Name utiva -Location EastUS

Create a VM

    New-AzVm -ResourceGroupName "Emmanuel"`
    -Name "vmpshell01"`
    -Location "East US"`
    -VirtualNetworkName "utivaVnet"`
    -SubnetName "pshellSubnet"`
    -SecurityGroupName "pshellNetworkSecurityGroup"`
    -PublicIPAddressName "pshellPublicIPAdress"`
    -OpenPorts 80,3389

## Azure function 

> nodejs function using http trigger
> to query from browser add this to the end.
    
    &name=utiva&location=uk

> to send a query.

### Basic contents from azure

----------

**index.js**

    module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const location = (req.query.location || (req.body && req.body.location));
    const responseMessage = name
        ? "Hello, " + name +" "+ location + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}

**function.json**

    {
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
 }


--------------
## ARM DEPLOYMENT USING POWERSHELL

> create new resource group

New-AzResourceGroup `
  -Name myResourceGroup `
  -Location "Central US"

------------
> create a cariable to contain path leading to .json ARM file

$templateFile = "C:\projects\Azure_Cloud_Developers\azuredeploy.json"

> Upload template .json file 

New-AzResourceGroupDeployment `
  -Name blanktemplate `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile

C:\projects\Azure_Cloud_Developers\azuredeploy.json

## Add VM to ASG

## Azure Identity Service

When you log into azure you use an identity username and password.

applications and apps utilize secret keys and certificates

> Authentication is the proccess of verification/assertion of identity 

> To manage azure active directory, you need to have a global administrator role.
etido@jehonoroutlook.onmicrosoft.com
etido | Xoqa5309 | Pufu0559
david | Pufu0559 | Vuha2122
smith | Vuha2122
emma | Wopa7810

> IAM --> Add+ ---> Role Assignment

## Policy

Policy > search "Location"

----

https://azure.microsoft.com/en-us/pricing/calculator/

https://azure.microsoft.com/en-us/pricing/tco/calculator/

https://azure.microsoft.com/en-us/support/legal/sla/


## Mount Disk on RHEL/Ubuntu

>lsvg
>pvcreate /dev/sdc
>vgcreate datavg /dev/sdc
>lvcreate  -L+5G -n datalv datavg
>mkfs --type xfs /dev/datavg/datalv
>mount /dev/datavg/datalv /data
>mkdir /data

Per4mance1234


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

## USING PAT to Login to git

> git remote set-url origin https://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@github.com/usianej/testdir.git