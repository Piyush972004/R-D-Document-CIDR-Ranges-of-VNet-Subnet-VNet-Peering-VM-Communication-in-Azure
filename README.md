# R-D-Document-CIDR-Ranges-of-VNet-Subnet-VNet-Peering-VM-Communication-in-Azure

# 🌐 Azure Virtual Network Peering Setup (Linux ↔ Windows VM)

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