# R-D-Document-CIDR-Ranges-of-VNet-Subnet-VNet-Peering-VM-Communication-in-Azure
---
## 📘 Introduction
An Azure Virtual Network (VNet) is a logically isolated network within Azure that enables resources to securely communicate with:
- Each other
- The Internet
- On-premises networks
It is a fundamental building block for constructing secure, multi-tier architectures on Azure.
---

---
## 📐 CIDR Ranges
CIDR (Classless Inter-Domain Routing) defines the IP address space using <IP address>/<prefix length> format.

##🔹 VNet CIDR
- Defines the total IP address space for the Virtual Network.

- Example: 10.0.0.0/16 provides 65,536 addresses.

- Ensure non-overlapping ranges when connecting to other VNets or on-premises networks.

## 🔹 Subnet CIDR
- Subnets use a portion of the VNet’s address space.

- Example: /29 provides 8 IPs, but only 3 usable due to Azure reserved addresses.

- CIDR     	IPs	   Usable IPs
- /29	      8	     3
- /24	      256	   251

## CIDR Limits
- Maximal: /0 – entire IPv4 space (4,294,967,296 addresses)

- Minimal: /32 – single IP address (very fine-grained control)
---

---
## 🧱 Subnets
A Subnet is a segment of a VNet's IP space that organizes and secures resources.

- Key Properties:
- Name: Unique within the VNet.

- CIDR Range: Must not overlap with other subnets.

- Reserved IPs: 5 IPs per subnet are reserved by Azure.

- IPv6 Support: Dual-stack (IPv4/IPv6) VNets are supported.

Features & Associations:
- Network Security Groups (NSGs): Control traffic at subnet level.

- NAT Gateway: For outbound internet traffic.

- Route Table: Custom routing rules.

- Service Endpoints: Direct access to Azure services.

- Delegation: Allow specific services to manage subnet resources.

## 🔗 VNet Peering
VNet Peering connects two or more VNets, allowing them to communicate over the Azure backbone network—bypassing public internet.

- ✨ Benefits:
- High bandwidth, low latency

- Private traffic (no encryption required)

- Supports cross-region and cross-subscription peering

- No downtime during peering setup

## 🧩 Types of VNet Peering
- Type	                         Description
- Intra-Region Peering	         Peers VNets within the same Azure region
- Global VNet Peering	           Connects VNets across different regions
- Cross-Subscription Peering	   Peers VNets from separate Azure subscriptions
- Cross-Tenant Peering	         Peers VNets from different Azure AD tenants
- Remote Gateway Peering	       Connects on-premise networks to peered VNets
- Transitive Peering	           Hub-and-spoke network setup
- Subnet-Level Peering	         Peer only selected subnets (granular control)

## ⚙️ Prerequisites for Creating a VNet
- To create a VNet in Azure:

- Azure Subscription: Ensure you have an active subscription.

- Permissions: Role like Network Contributor or custom permissions with:

- Microsoft.Network/virtualNetworks/*

- Microsoft.Network/virtualNetworks/subnets/write

- IP Address Planning: Plan CIDR ranges to avoid overlaps.

- Resource Group: Select/create a Resource Group to host the VNet.

- Region Selection: Choose the appropriate Azure region for deployment.



##  🌐 Azure Virtual Network Peering Setup (Linux ↔ Windows VM)

This project demonstrates how to:
- Create VNets and Subnets with custom CIDR ranges
- Launch Linux and Windows VMs in separate VNets
- Configure NSGs to allow RDP, SSH, and ICMP
- Establish VNet Peering between the two VNets
- Verify connectivity via `ping` between VMs

## 📌 CIDR Ranges

| VNet   | CIDR Range   | Subnet        |
|--------|--------------|---------------|
| vnet1  | 10.1.0.0/16  | default       |
| vnet2  | 10.2.0.0/16  | subnet2       |

## 🔧 VM Configuration

| VM       | OS                       | Size             | Username | Auth Type |
|----------|--------------------------|------------------|----------|-----------|
| LinuxVM  | Ubuntu 22.04             | B2as v2          | Piyush   | SSH Key   |
| WinVM    | Windows Server 2019      | B2as v2          | azureuser| Password  |

## 🔐 NSG Rules

- **Linux NSG**
  - Allow SSH (Port 22)
  - Allow ICMP (Ping)
- **Windows NSG**
  - Allow RDP (Port 3389)
  - Allow ICMP (Ping)

## 🔗 VNet Peering

- Peering from `vnet1` to `vnet2` and vice versa
- Enabled:
  - Allow traffic between VNets
  - Allow forwarded traffic

## ✅ Connectivity Test

Successfully tested:
```bash
ping 10.1.0.4  # From LinuxVM to WinVM
