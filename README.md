# Azure-VM-Public-IP-Upgrade-Basic-SKU-to-Standard-SKU

## Project Overview

This project provides solutions for upgrading Azure Virtual Machine (VM) public IP addresses from Basic SKU to Standard SKU, addressing Microsoft's advisory about the upcoming retirement of Basic SKU public IP addresses by September 30, 2025.
As part of Microsoft's deprecation of **Basic SKU Public IP Addresses**, this project documents the process of upgrading over 300+ Azure Virtual Machines from **Basic** to **Standard SKU** Public IPs. The project includes both **manual** and **automated** methods to perform the upgrade securely and with minimal service interruption.

> ⏰ **Deadline:** Basic SKU Public IPs will be retired on **30 September 2025**  
> 📧 Notification Reference: Azure Advisory — Tracking ID `6TH2-BV8`

## 📢 Microsoft Advisory Summary

- **Basic SKU Public IPs** will no longer be supported after **30 Sept 2025**
- Creation of new Basic SKUs was disabled as of **31 March 2025**
- **Standard SKU** provides improved:
  - Integration with Azure Firewall, Load Balancer, NAT Gateway
  - Security defaults (closed to inbound by default)
  - Zonal and zone-redundant availability

🔗 [Microsoft Documentation – Public IP SKU](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses#sku)

---
