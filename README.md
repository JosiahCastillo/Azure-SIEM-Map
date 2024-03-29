# Azure-SIEM-Map (Asset descriptions coming soon)

## Objective

The purpose of this lab is to deploy a honeypot in the Microsoft Azure cloudspace and feed live international cyber attack data into a Securtiy Information and Event Management (SIEM) system. Once everything is deployed, the architecture should follow the diagram provided below.
![Azure Network Diagram](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/6cb0e379-2629-4bc1-bcd5-176284d53e4c)


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

### Step 1: Deploying the Honeypot
To deploy a honeypot, we will need to deploy a virtual machine (VM) in an Azure Resource Group and take the first step toward making it accessible from all over the internet. We will accomplish this by adding custom rules to our network security group (NSG), which will allow all network traffic to pass through the NSG to the VM.

Firstly, we'll need to navigate to the Virtual Machines service and create a virtual machine. You should be greeted with this window.

![Deploying_the_Honeypot_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/f2ad3a84-c01b-450a-9227-68d38cb24f23)

To deploy our resources within our Azure subscription, we'll need to create a resource group. We can do this by navigating to the "Resource group" field located under "Subscription". Select "Create new" and enter a name for your resource group. In my case, I named my resource group HoneypotLab.

![Deploying_the_Honeypot_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/ae69c940-5860-43d4-8451-bd69b47e4eeb)
![Deploying_the_Honeypot_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/6dde6012-1ef9-4192-aa87-81e73cfcd777)
![Deploying_the_Honeypot_4](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/56cbef0f-d74e-465e-a795-14e85e00f6c5)
![Deploying_the_Honeypot_5](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/dca734b4-0204-41f7-b2fb-6efad659a7d3)
![Deploying_the_Honeypot_6](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/e18e8d1e-6eae-42e2-99e6-2539a8abe8a9)

### Step 2: Creating a Log Analytics Workspace
The Log Analytics Workspace allows us to query logs from any VMs we connect to it, and have them presented to us in an organized form on the Azure platform. Later, we can use this workspace to feed logs into our Security Information and Event Management (SIEM) system by querying custom tables constructed from these logs. 

![Creating_a_Log_Analytics_Workspace_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/142ee21d-3d06-4449-910f-c75620f0b4e6)
![Creating_a_Log_Analytics_Workspace_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/51b71020-e214-4260-91af-c405a9ccab2b)


### Step 3: Enabling Log Retrieval
In order for us to retrieve logs from out honeypot VM, we will need to enable this event collection feature through Microsoft Defender. This will require us to activate Microsoft Defender on our Azure subscription and enable the Server plan. This will give us the option to enable us to store all Windows security event data from our honeypot in our Log Analytics Workspace.

![Enabling_Log_Retrieval_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/ca28d642-5d62-4e20-bfe9-f2741d880036)
![Enabling_Log_Retrieval_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/5fce1746-dbc6-4bd0-a06c-ba0c7bde0e5d)
![Enabling_Log_Retrieval_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/17ef1753-aafe-43e2-a51d-1f24e1390668)


### Step 4: Accessing the Honeypot
To access our honeypot we will need to use the Remote Desktop Protocol (RDP) to remotely login to our deployed VM. After we gain access to the machine, we will become familiar with Windows Event Manager security logs, and ensure our VM can be pinged by remote scans across the internet.

![Accessing_the_Honeypot_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/e8d628b5-ec8a-456c-ac89-94359fb7098d)
![Accessing_the_Honeypot_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/0d61b209-ccb2-429c-b6f5-66731528da16)
![Accessing_the_Honeypot_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/d8bd7f08-1d65-40b5-b7e9-81c104850edf)
![Accessing_the_Honeypot_4](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/02da5a20-92a9-4969-b2d3-9153c73be7e4)
![Accessing_the_Honeypot_5](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/d612cddf-3355-4284-9588-e603b6d79c22)
![Accessing_the_Honeypot_6](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/c757e51c-26f7-4c08-b4cc-d81d65ffc817)

### Step 5: Log Generation
We will need to generate logs containing geolocation information in order for attacks to be plotted on a world map. To accomplish this, we will be feeding the IP addresses found in the Windows Event Manager security logs to an IP Geolocation API. This will return the longitude and latitude associated with the IP. We will then use this along with security logs to generate custom logs.

![Log_Generation_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/e9f7072d-028f-4fa5-883d-9f2beafefbdd)
![Log_Generation_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/01be90e3-984e-4bb2-a89d-ed8e9841afd8)


### Step 6: Log Extraction
To extract the custom logs from our honeypot VM we will need to create a custom MMA table in our Log Analytics Workspace. We will then need to provide a sample log for the workspace to parse along with the original log's pathway on our honeypot VM in order to populate the table. Afterwards, we should be able to query our custom logs freely.

![Log_Extraction_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/db36bfb6-434e-447a-8962-2ae46e63e89c)
![Log_Extraction_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/a9cb7d3e-d871-41d1-98eb-860265ff0f97)
![Log_Extraction_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/d4f2a249-6aa9-471c-907a-a176529883ff)
![Log_Extraction_4](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/6be66310-8d6e-494c-86fd-8b832d725d6d)
![Log_Extraction_5](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/9f8b80cc-7546-4c87-9b56-f35a848fdbed)
![Log_Extraction_6](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/18d308fb-101c-4136-8434-935c7e5b264f)


### Step 7: SIEM Configuaton
To visualize our attack data on a world map we will be using the built-in map functionality in Azure's SIEM tool, Azure Sentinel. To accomplish this task, we will need to connect our Log Analytics Workspace to Azure Sentinel and use Kusto Query Language (KQL) to parse custom fields. Once parsed, we can configure the Sentinel Workbook to vizualize location data according to whatever fields we choose to extract.

![SIEM_Configuration_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/035ea18d-80e0-4976-b3ab-0e3f9d4b8c5b)
![SIEM_Configuration_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/9116e757-ec36-47ed-bbfd-3661c456a631)
![SIEM_Configuration_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/87a0726d-967a-4965-acee-be0447ae06dd)
![SIEM_Configuration_4](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/611003e3-1a80-47f5-aa8a-260a7bf9faf0)
![SIEM_Configuration_5](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/c6579370-fae5-4221-ab76-ef302c67ccf5)


## Results
After some time we should be able to see failed RDP attempts logged on our world map. The following are results from my own implementation.

![Results_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/f372fa72-cbd3-4490-aa17-29fd98a7642e)
![Results_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/236a0f5b-27d4-4a8c-aca6-b1a6a6f68f2a)

