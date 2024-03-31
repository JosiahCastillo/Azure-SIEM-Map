# Azure-SIEM-Map (Asset descriptions in progress)

## Objective

The purpose of this lab is to deploy a honeypot in the Microsoft Azure cloudspace and feed live international cyber attack data into a Securtiy Information and Event Management (SIEM) system. Once everything is deployed, the architecture should follow the diagram provided below. While not explicitly dependent on a resource group, the subscription is meant to indicate its involvement in enabling data storage policies in Microsoft Defender for Cloud.

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

Firstly, I'll need to navigate to the Virtual Machines service and create a virtual machine.

![Deploying_the_Honeypot_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/f2ad3a84-c01b-450a-9227-68d38cb24f23)

To deploy my resources within my Azure subscription, I'll need to create a resource group. I did this by navigating to the "Resource group" field located under "Subscription". In my case, I named my resource group HoneypotLab, and I'll be using this resource group for the rest of the services deployed for this lab.

![Deploying_the_Honeypot_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/ae69c940-5860-43d4-8451-bd69b47e4eeb)

Next, I entered the name for my VM, Honeypot, and selected the region I'll be deploying in, East US. The region is important, because I'll be using it for other services. I also made sure my region had the "Windows 10 Pro" image available as I've shown in the image above. Regions can vary on availability so this can come into play later on when selecting storage volumes.

![Deploying_the_Honeypot_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/6dde6012-1ef9-4192-aa87-81e73cfcd777)

When selecting size, I went with a "Standard_B1s" size since it was free services eligible. At this point I also determined the Administrator account's username and password, which I'll be using later to RDP into the honeypot. I'll then move towards the "Public inbound ports" field and make sure "Allow selected ports" is selected. The "Select inbound ports" field should be set to RDP so we can remotely log into the VM later.  After selecting the Licensing field, I move on to networking settings.

![Deploying_the_Honeypot_4](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/56cbef0f-d74e-465e-a795-14e85e00f6c5)

In this tab, I created a virtual network for my honeypot and a network security group to enable the VM to be discovered. Most of the settings were generated by default do the only thing I really needed to do was to make sure the "NIC network security group" field was set to "Advanced". Next, I selected "Create new" under the "Configure network security group" field.

![Deploying_the_Honeypot_5](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/dca734b4-0204-41f7-b2fb-6efad659a7d3)

In this menu, I removed all prexisting outbound rules and entered a rule named "ANY_IN". This rule allows any source to send traffic from any port ranges through the NSG to any port ranges at any destination. I set any protocol to be allowed which is obviously not best practice, but since this is a honeypot and I really wanted it to be discoverable, I went ahead and implemented it. I also set the priority to 100 since the rules with the lowest priority take precedence.

![Deploying_the_Honeypot_6](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/e18e8d1e-6eae-42e2-99e6-2539a8abe8a9)

After configuring the NSG, I make sure to check the field that deletes the allocated public IP and NIC when the VM is deleted. Everything else in the image should remain the same, so I'll review and create. This should get my honeypot VM up and running.

### Step 2: Creating a Log Analytics Workspace
The Log Analytics Workspace allows us to query logs from any VMs we connect to it, and have them presented to us in an organized form on the Azure platform. Later, we can use this workspace to feed logs into our Security Information and Event Management (SIEM) system by querying custom tables constructed from these logs. 

This is the Log Analytics Workspace console after selecting "Create Log Analytics Workspace".

![Creating_a_Log_Analytics_Workspace_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/142ee21d-3d06-4449-910f-c75620f0b4e6)

In this menu, I made sure the "HoneypotLab" resrouce group I created is selected in the "Resource group" field. Afterwards I simply named the LAW. In my case, I code something simple like "Honeypot-Law". I also made sure the "Region" field had the "East US" region selected since that was the region where I deployed my honeypot VM.

![Creating_a_Log_Analytics_Workspace_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/51b71020-e214-4260-91af-c405a9ccab2b)

After creating the honeypot LAW, I needed to connect it to my honeypot VM. This is essentially what will allow logs to be queried from the VM after its log collection policy is activated via the Microsoft Defender for Cloud service. This will eventually enable me to feed the tables containing the custom logs to an Azure Sentinel workbook.


### Step 3: Enabling Log Retrieval
In order for us to retrieve logs from out honeypot VM, we will need to enable this event collection feature through Microsoft Defender. This will require us to activate Microsoft Defender on our Azure subscription and enable the Server plan. This will give us the option to enable us to store all Windows security event data from our honeypot in our Log Analytics Workspace.

Firstly, I navigated to Microsoft Defender for Cloud, enabled it in my Azure subscription, then navigated to the Environment settings menu you see here.

![Enabling_Log_Retrieval_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/ca28d642-5d62-4e20-bfe9-f2741d880036)

In this menu you can also see the LAW I created listed undeneath my Azure subscription. Currently I have 0 plans active in my subscription and my LAW. Since the LAW is connected to my VM from the last section, I should be able to activate a Server Defender Plan once I select Honeypot-Law from the Environment settings dropdown.

![Enabling_Log_Retrieval_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/5fce1746-dbc6-4bd0-a06c-ba0c7bde0e5d)

After selecting Honeypot-Law from the dropdown, I set the Servers plan to On, and save my settings. This should lock in the Foundational CSPM plan to On, and since I do not need any services for SQL servers, I left that policy Off.

![Enabling_Log_Retrieval_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/17ef1753-aafe-43e2-a51d-1f24e1390668)

Now that server policies are enabled, I navigate to Data collection settings. By selecting "All Events", this allows for all Windows security events and VM data to be stored in my LAWs with the server policy enabled. Since I will be generating my custom logs from the Windows Event Viewer security logs and extracting them to a custom table in Honeypot-Law, this data storage service is a necessary feature to enable.


### Step 4: Accessing the Honeypot
To access our honeypot we will need to use the Remote Desktop Protocol (RDP) to remotely login to our deployed VM. After we gain access to the machine, we will become familiar with Windows Event Manager security logs, and ensure our VM can be pinged by remote scans across the internet.

For this stage in the lab I'll need to grab my VM's public ip address. 

![Accessing_the_Honeypot_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/e8d628b5-ec8a-456c-ac89-94359fb7098d)

To do this, I navigated to the Virtual Machine service and selected my honeypot. Then, on the right side under "Essentials", I copied the honeypot's public IP. Next, I use the IP to RDP into my honeypot to I can start exposing the VM to the internet. When loggin in, I also had to use my admin credentials from step 1 part 3.

![Accessing_the_Honeypot_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/0d61b209-ccb2-429c-b6f5-66731528da16)

Once inside the VM, I'll need to run through some basic setup involving privacy settings and connectivity. I don't login to any accounts or enable any services on this machine since its unnecesary for the lab. When the desktop loads, I also allow my machine to be discoverable for all devices on the network, but this isn't really necessary.

![Accessing_the_Honeypot_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/d8bd7f08-1d65-40b5-b7e9-81c104850edf)

To gather a bit more insight as to what kind of logs I'll need to be filtering for, I navigate to the Windows Event Viewer, and go to "Windows Logs" and "Security". After some time, I'm able to see a list off all logged security events each with their respective Event IDs and metadata. Here, I determine the Event ID associated with failed RDP attempts is 4625. Later, I'll be using this Event ID to filter for these specific logs through a PowerShell script.

![Accessing_the_Honeypot_4](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/02da5a20-92a9-4969-b2d3-9153c73be7e4)

Before this, I test to see if my honeypot is properly exposed to the internet by attempting to ping the VM from my own local machine. Initially, my pings were unsuccessful likely due to the Windows Defender Firewall blocking inbound ICMP requests.

![Accessing_the_Honeypot_5](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/d612cddf-3355-4284-9588-e603b6d79c22)

To solve this, I disabled the firewall even though I could've adjusted specific inbound rules for ICMP IPv4 and IPv6. I did this by setting the "Firewall state" field to "Off" for Domain, Private, and Public profiles. While this obviously isn't best practice, for the purposes of this lab, it should serve as a sufficient solution. If I ever do revisit this project, I will likely attempt to log other types of intrusion attempts and implement more refined traffic filtering.

![Accessing_the_Honeypot_6](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/c757e51c-26f7-4c08-b4cc-d81d65ffc817)

Once the Windows Defender Firewall was disabled, I was able to discover my honeypot. If this hadn't worked, I would likely need to check the rules on my Network Security Group. Now that my VM is discoverable, I move on to generating custom logs.

### Step 5: Log Generation
We will need to generate logs containing geolocation information in order for attacks to be plotted on a world map. To accomplish this, we will be feeding the IP addresses found in the Windows Event Manager security logs to an IP Geolocation API. This will return the longitude and latitude associated with the IP. We will then use this along with security logs to generate custom logs.

I used PowerShell to generate custom logs and the file they're contained in. The script used in this lab can be found in the Scripts and Code directory in this repository.

![Log_Generation_1](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/e9f7072d-028f-4fa5-883d-9f2beafefbdd)

To use the PowerShell script, I used the run menu (Windows + R) to open the PowerShell ISE and opened the script. Firstly, the code checks the Windows Event Viewer security logs and filters them according to the 4625 Event ID, which refers to failed RDP authentication attempts. Next, the script gathers the IP addresses from these logs. The code then uses an API key for the Ipgeolocation API to gather geolocation information after providing an IP addresses it extracted from the security logs. Afterwards, it creates a log file called "failed_rdp.log" and enters values containing the geolocation data returned from the API along with some other custom fields.

![Log_Generation_2](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/c44ebf45-3fa2-46e4-8515-6f71bad54ae9)

In order to use the Ipgeolocation API, I registered an account and navigated to the dashboard as shown above. As mentioned earlier, the script used in this lab uses an API key which is attainable at the Ipgeolocation dashboard. Once I copied and pasted my API key, I was able to run the script and generate custom logs based off of previous intrusion attempts.

![Log_Generation_3](https://github.com/JosiahCastillo/Azure-SIEM-Map/assets/47875741/607f9cc3-13f8-43dc-8059-4f0a07b7a803)

Once the script is running, it prints out the new custom log entries to the console, and stores a copy of the log entries into the "failed_rdp.log" file. For the rest of the lab, I left this script running so it can gather live intrusion data as it comes in. From this point on, I didn't need to access the VM, so I ended my RDP session and changed the administrator credentials to something reasonably difficult to brute force. The credentials were already fairly strong, but this is just an extra step I took.


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

