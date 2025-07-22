# Azure-VM-Public-IP-Upgrade-Basic-SKU-to-Standard-SKU
## Project Overview
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
## Solutions

Manual Upgrade Process

The manual process involves:

Checking IP assignment type (static/dynamic)

Shutting down the VM (if applicable)

Dissociating the public IP (if needed)

Changing dynamic IPs to static

Performing the SKU upgrade

Reassociating the IP (if previously dissociated)

Powering the VM back on

🧭 Manual Upgrade Process
Step 1: Validate IP Assignment
Navigate to Public IP Resource > Configuration

Check if the IP is Static or Dynamic

Step 2: Graceful Shutdown
Shutdown VM from OS, avoid full deallocation

Step 3: Dissociate Public IP
Go to VM > Networking > NIC > IP Configuration

Click the IP interface (e.g., ipconfig1)

Uncheck "Associate public IP" and Save

Step 4: Set IP to Static (if Dynamic)
In Public IP Resource > Configuration, change IP Assignment to Static

Step 5: Upgrade SKU
Navigate to the Public IP

Click banner “Upgrade to Standard SKU”

Confirm upgrade (no rollback option)

Verify SKU in Overview tab as Standard

Step 6: Reassociate Public IP
Go back to VM > NIC > IP Configuration

Re-associate the upgraded Standard Public IP

Step 7: Power On and Test
Start the VM and confirm connectivity

Automated Upgrade Solution
The PowerShell script Start-VMPublicIPUpgrade.ps1 automates the upgrade process with these features:

Checks current SKU status (skips if already Standard)

Handles both static and dynamic IP addresses

Creates recovery logs for tracking

Supports WhatIf mode for dry runs

Getting Started
Prerequisites
Azure PowerShell module

Appropriate Azure RBAC permissions

PowerShell 5.1 or later

Usage
powershell
# Single VM upgrade (dry run)
Start-VMPublicIPUpgrade -VMName 'vmname' -ResourceGroupName 'rgname' -WhatIf

# Actual upgrade
Start-VMPublicIPUpgrade -VMName 'vmname' -ResourceGroupName 'rgname'

# Bulk upgrade (pass array of VM names)
$vmList = @('vm1','vm2','vm3')
$vmList | ForEach-Object {
    Start-VMPublicIPUpgrade -VMName $_ -ResourceGroupName 'rgname'
}
Important Notes 🔐 Notes on Security and Availability
Service Impact: The upgrade process may cause brief network interruptions

Irreversible Action: Once upgraded to Standard SKU, you cannot revert to Basic SKU

Testing: Always test in non-production environments first

Backup: Ensure you have proper backups before making changes

Downtime: Minor disruption may occur during disassociation/reassociation.

No Downgrade: Once upgraded to Standard SKU, the Public IP cannot be downgraded.

Capacity Planning: Avoid deallocation unless capacity is guaranteed.

Automation Fallback: Script safely skips already-upgraded SKUs.



References
🔗 [Microsoft Documentation – Public IP SKU](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses#sku)
🔗 [Upgrade public IP addresses attached to VM from Basic to Standard](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-upgrade-vm?tabs=azure-cli)
🔗 [AzureVMPublicIPUpgrade 1.0.2]([https](https://www.powershellgallery.com/packages/AzureVMPublicIPUpgrade/1.0.2))
🔗 [AzureVMPublicIPUpgrade.psm1]([https](https://www.powershellgallery.com/packages/AzureVMPublicIPUpgrade/1.0.2/Content/AzureVMPublicIPUpgrade.psm1))
🔗 [Service Health](https://portal.azure.com/#view/Microsoft_Azure_Health/AzureHealthBrowseBlade/~/serviceIssues)
🔗 [Service Health Advisories ]([https://portal.azure.com/#view/Microsoft_Azure_Health/AzureHealthBrowseBlade/~/serviceIssues](https://portal.azure.com/#view/Microsoft_Azure_Health/AzureHealthBrowseBlade/~/otherAnnouncements))
🔗 [Microsoft Advisory Notice](https://app.azure.com/h/6TH2-BV8/a96207)
🔗 [Services Retirement Workbook](https://portal.azure.com/#view/AppInsightsExtension/UsageNotebookBlade/ComponentId/Azure%20Advisor/ConfigurationId/community-Workbooks%2FAzure%20Advisor%2FAzureServiceRetirement/Type/workbook/WorkbookTemplateName/Service%20Retirement%20(Preview))

