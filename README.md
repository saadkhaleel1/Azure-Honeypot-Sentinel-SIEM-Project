<!--# Azure-SOC-Honeynet-Project-->
# Azure-Honeypot-Sentinel-SIEM-Project
![SOC Honeynet (1)](https://github.com/0xbythesecond/Azure-SOC-Honeynet-Project/assets/23303634/43177fa9-4746-4f8d-8774-f9aca74b891d)

![SOC Honeynet (1)](https://imgur.com/Cl0HRnH.png)
## Introduction
I present a summary of varying parts to create a HoneyNet via Microsoft Azure. This HoneyNet is to provide a visual representation of real-world cyber attacks from all parts of the world. The HoneyNet is designed to allow me to gather data related to the different bad actors from across the world from differing IP addresses.

## Sub-Intro
In this project, I build a mini HoneyNet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measured metrics for another 24 hours, then show the results below. 


## Azure Resources Deployed, Technologies, and Regulations used:
- [Azure Virtual Network](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) (VNet)
- [Azure Network Security Group](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview) (NSG)
- [Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/overview) (2x Windows 10 Pro, 1x Linux Server)
- [Log Analytics Workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-workspace-overview) with Kusto Query Language (KQL) Queries
- [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/basic-concepts) for Secure Secrets Management
- [Azure Storage Account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview) for Data Storage
- [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview) for Security Information and Event Management (SIEM)
- [Microsoft Defender](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-cloud-introduction) for Cloud to Protect Cloud Resources
- [Windows Remote Desktop](https://support.microsoft.com/en-us/windows/how-to-use-remote-desktop-5fe128d5-8fb1-7a23-3b8a-41e636865e8c) for Remote Access
- [Command Line Interface](https://www.w3schools.com/whatis/whatis_cli.asp) (CLI) for System Management
- [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.3) for Automation and Configuration Management
- [NIST SP 800-53 Revision 4](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance

## Course of Action

1. **Setting up Azure Services for Security:** I created a project utilizing Azure services for Security Information and Event Management (SIEM), using Azure as a honeypot, and leveraging Sentinel for security monitoring.

2. **Intentional Vulnerability:** Deliberately leaving firewalls open to invite potential attacks from unknown users globally, aiming to gather diverse threat intelligence and attack patterns.

3. **Creating a Virtual Machine:** I deployed a virtual machine in Azure, configuring it with appropriate settings, likely an operating system, size, networking, and administrative credentials.

4. **Connecting VM to Log Analytics:** I established a connection between the Virtual Machine and your Log Analytics workspace in Azure, enabling the collection and storage of logs and telemetry data generated by the VM.

5. **Observing Event Viewer Logs:** Within the VM, I observed Event Viewer logs, intentionally turned off the Windows Firewall, downloaded a PowerShell script, obtained a geolocation IO API key, and executed the script to gather geodata from potential attackers for geolocation tracking.

6. **Custom Log Integration:** Integrated custom logs into Azure Sentinel, created custom fields using KQL to extract specific information from raw custom log data, and tested the extraction to ensure accuracy and functionality.

7. **Workbook Creation:** Created a new workbook in Azure Sentinel, configuring a map visualization with latitude and longitude data extracted from geolocation information or relevant datasets. Adjusted plot sizes on the map for clear visualization.


The metrics we will show are:
- Powershell (Code)
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Security Controls
![Architecture Diagram](https://imgur.com/YtSwYoV.png)
In the "BEFORE" measurement phase, The project architecture utilizes Azure services for SIEM and a honeypot, leveraging Sentinel for security monitoring, while intentionally leaving firewalls open to invite global unknown users' attacks. This strategy enables the collection of diverse threat intelligence and attack patterns for analysis and informs the subsequent hardening of security controls to fortify the system against potential vulnerabilities identified through the observed attacks.

## Created A VM / Security Controls
![Architecture Diagram](https://imgur.com/V7xOgnB.png)
In the "AFTER" evaluation stage, I began by accessing the Azure portal and navigating to the "Virtual Machines" section. After selecting "Create," I provided details such as subscription, resource group, region, and chose an appropriate operating system image. Then, during the configuration process, I ensured to enable diagnostics settings and select "Send to Log Analytics" while specifying the Log Analytics workspace where I wanted the VM's logs and telemetry data to be directed. Once the VM was successfully deployed, I verified its connection to the Log Analytics workspace by checking the diagnostics settings and confirming that the data generated by the VM was being collected and stored within the specified Log Analytics workspace for further analysis and monitoring via Azure Sentinel or other security analysis tools.

## Used Powershell  / Security Controls
To observe the Event Viewer logs in the Virtual Machine (VM) and facilitate geolocation tracking of potential attackers, I initiated by accessing the VM and navigating to the Event Viewer tool within the Windows operating system. Subsequently, I disabled the Windows Firewall on the VM to intentionally expose potential vulnerabilities. Following this, I downloaded a PowerShell script designed to access a geolocation IO API and collect geodata from incoming connections and attempted attacks. After obtaining the API key for the geolocation service, I ran the PowerShell script on the VM. This script was configured to capture and extract geolocation data from incoming connections made by potential attackers, enabling the identification of their geographic origins. By correlating this geodata with observed attack patterns and Event Viewer logs, I aimed to gather insights into the geographic locations of these attackers, enhancing the understanding of the source and distribution of potential threats targeting the VM.
![MSSQL Allowed Access](https://imgur.com/jEDtMYp.png) <br />

To integrate custom logs into Azure Sentinel for analysis, I initiated the process by defining a custom log source. I navigated to Azure Sentinel and accessed the Data Connectors section, where I selected the option to create a custom log connector. Then, I specified the format and details of the custom log, including the data source type, log format, and connection parameters.

Once the custom log was integrated, I proceeded to create custom fields within Azure Sentinel to extract specific information from the raw custom log data. Using KQL (Kusto Query Language), I crafted extraction queries to define and parse out distinct fields containing relevant information. These custom fields were tailored to extract essential data elements, enabling more granular analysis and query capabilities within Azure Sentinel.

To ensure the accuracy and functionality of the custom field extraction, I conducted thorough testing. This involved running test queries against the raw custom log data within Azure Sentinel to validate that the created custom fields successfully extracted the intended information. Through this testing phase, I verified the accuracy of the custom field extraction process, ensuring that the extracted fields contained the expected data from the raw custom log entries. This validated the efficacy of the custom log integration and field extraction processes within Azure Sentinel for further analysis and monitoring of the specific log data.
![Linux Syslog Auth Failures](https://imgur.com/bW0vCJb.png) <br />

I accessed the Azure Sentinel workspace and navigated to the Workbooks section. Here, I initiated the creation of a new workbook and selected the map visualization option.

Within the workbook editor, I configured the map visualization by incorporating the latitude and longitude fields extracted from the collected geolocation data or any relevant dataset containing location-based information. Using KQL queries or data selection options within the workbook, I specified the latitude and longitude fields to plot the geographic locations of the attackers or relevant data points on the map.

To fix the plot sizes on the map, I adjusted the visualization settings within the workbook editor. This involved modifying the map properties, such as zoom levels, markers, and sizing options, to ensure clear and appropriately sized plots for the data points. By customizing these settings, I optimized the map visualization to accurately display the geographical distribution of attackers or specific data locations in a clear and understandable manner within the Azure Sentinel workbook.

This enabled a comprehensive view of the geographic distribution of identified threats or relevant data points, providing valuable insights into the global reach and concentration of potential attacks or specified data locations within the Azure Sentinel environment.
![Windows RDP/SMB Auth Failures](https://imgur.com/Qh3txiP.png) <br />

The illustrated attack map serves as a compelling showcase of the ramifications stemming from the act of leaving the Network Security Group (NSG) unrestricted, thereby facilitating the unhindered ingress of malicious network traffic. This visualization effectively emphasizes the criticality of deploying robust security protocols, including the imposition of stringent NSG rules, as a means to thwart unauthorized entry and mitigate the inherent risks posed by potential threats.
![NSG Allowed Inbound Malicious Flows](https://imgur.com/dzlgBsi.png)

## Reflection
This has been both a challenging and rewarding experience creating this lab and how real-world traffic can be analyzed using attack maps as well as KQL data to parse out different metrics. It was a beautiful sight to see everything come together and have the ability to paint a picture of an insecure environment as well as one that is secure and you no longer see the malicious traffic after implementing the various security controls. During the process of leaving the resources vulnerable, I was able to see the differing IP addresses of the bad actors and the user names that they were attempting to access my virtual machines. After the hardening was completed and waiting 24 hours, it was quite a sight to behold when seeing that there were 0 results found that represent any allowed traffic from the bad actors on the public internet.

## Conclusion
This project involved the establishment of a mini honeynet within the Microsoft Azure platform, where diverse log sources were seamlessly integrated into a dedicated Log Analytics workspace. Microsoft Sentinel played a pivotal role in proactively generating alerts and initiating incidents based on the logs ingested. Notably, comprehensive metrics were diligently measured in the vulnerable environment prior to the implementation of security controls, followed by a subsequent assessment after fortifying the infrastructure. The remarkable outcome emerged as a significant reduction in the frequency of security events and incidents, which undeniably attested to the efficacy of the implemented security measures.

It is important to acknowledge that if the network's resources were extensively utilized by regular users, it is conceivable that a greater number of security events and alerts could have been generated within the 24-hour timeframe subsequent to the enforcement of the security controls.
