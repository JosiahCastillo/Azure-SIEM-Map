# Azure-SIEM-Map

## Objective

The purpose of this lab is to deploy a honeypot in the Microsoft Azure cloudspace and feed live international cyber attack data into a Securtiy Information and Event Management (SIEM) system.

## Skills Learned

- Azure console
- SIEM functionality (Azure Sentinel)
- Network Security Groups (NSGs)
- Powershell Scripting
- Kusto Query Language (KQL)

## Tools Used

- Azure Sentinel
- Azure Log Analytics Workspace
- Azure Virtual Machines
- Azure Network Security Groups
- Microsoft Defender
- Powershell
- Secure Web Protocols
- Windows Operating System

## Key Steps

### Deploying the Honeypot
To deploy a honeypot, we will need to deploy a virtual machine (VM) in an Azure Resource Group and take the first step toward making it accessible from all over the internet. We will accomplish this by adding custom rules to our network security group (NSG), which will allow all network traffic to pass through the NSG to the VM.

### Creating a Log Analytics Workspace
The Log Analytics Workspace allows us to query logs from any VMs we connect to it, and have them presented to us in an organized form on the Azure platform. Later, we can use this workspace to feed logs into our Security Information and Event Management (SIEM) system. 

### Enabling Log Retrieval
In order for us to retrieve logs from out honeypot VM, we will need to enable this event collection feature through Microsoft Defender.

### Accessing the Honeypot
### Log Generation
### Log Extraction
### Log Retrieval
### SIEM Configuaton
### Results
