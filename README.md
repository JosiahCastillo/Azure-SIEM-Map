# Azure-SIEM-Map (Detailed Steps Coming soon)

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
In order for us to retrieve logs from out honeypot VM, we will need to enable this event collection feature through Microsoft Defender. This will require us to activate Microsoft Defender on our Azure subscription and enable the Server plan. This will give us the option to enable us to store all Windows security event data from our honeypot in our Log Analytics Workspace.

### Accessing the Honeypot
To access our honeypot we will need to use the Remote Desktop Protocol (RDP) to remotely login to our deployed VM. After we gain access to the machine, we will become familiar with Windows Event Manager security logs, and ensure our VM can be pinged by remote scans across the internet.

### Log Generation
We will need to generate logs containing geolocation information in order for attacks to be plotted on a world map. To accomplish this, we will be feeding the IP addresses found in the Windows Event Manager security logs to an IP Geolocation API. This will return the longitude and latitude associated with the IP. We will then use this along with security logs to generate custom logs.

### Log Extraction
To extract the custom logs from our honeypot VM we will need to create a custom MMA table in our Log Analytics Workspace. We will then need to provide a sample log for the workspace to parse along with the original log's pathway on our honeypot VM in order to populate the table. Afterwards, we should be able to query our custom logs freely.

### SIEM Configuaton
To visualize our attack data on a world map we will be using the built-in map functionality in Azure's SIEM tool, Azure Sentinel. To accomplish this task, we will need to connect our Log Analytics Workspace to Azure Sentinel and use Kusto Query Language (KQL) to parse custom fields.

### Results


