
Azure Cloud Security Operations Center (SOC) & Honeypot
📝 Objective
This project demonstrates the setup of a basic Security Operations Center (SOC) in the cloud using Microsoft Azure. I configured a virtual machine to act as a honeypot, intentionally exposing it to the public internet to attract malicious actors. I then monitored, collected, and forwarded the attack logs to a centralized repository (Log Analytics Workspace) and utilized Microsoft Sentinel (a cloud-native SIEM) to map the geographic origins of the live attacks in real-time.

🛠️ Tools & Technologies Used
Microsoft Azure: Cloud infrastructure provider.

Virtual Machines (Ubuntu): Served as the honeypot.

Azure Network Security Groups (NSG): Used to intentionally configure a highly vulnerable firewall.

Remote Desktop Protocol (RDP): Used to access and configure the honeypot.

Log Analytics Workspace: Centralized log repository for the ingested security events.

Microsoft Sentinel (SIEM): Used to process, query, and visualize the incoming attack logs.

Kusto Query Language (KQL): Used to query the logs and filter for specific security events (Event ID 4625).

PowerShell/Event Viewer: Local monitoring of Ubuntu Security logs.

🗺️ Architecture Overview
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ca576396-a4b2-4434-8821-36d149363455" />

The architecture consists of a public-facing Ubuntu VM intentionally left vulnerable to the internet. Logs are routed via an Azure Monitor Agent to a Log Analytics Workspace, where Microsoft Sentinel enriches the data using a GeoIP Watchlist to plot attackers on a geographic map.

🚀 Step-by-Step Implementation
Step 1: Create the Azure Infrastructure
Created a free Microsoft Azure subscription.

Created a Resource Group (e.g., rg-socklab) to house all project assets.

Deployed a Virtual Network (VNet) and subnet inside the Resource Group.

Deployed a Ubuntu Virtual Machine into the VNet.

Placeholder: Take a screenshot of your Azure Portal showing your created Resource Group and deployed VM.

Step 2: Expose the Honeypot to the Internet
To ensure the VM receives traffic (attacks) from malicious bots scanning the internet:

Navigated to the Network Security Group (NSG) associated with the VM.

Deleted default inbound firewall rules.

Created a new inbound rule (named "Danger") allowing ANY traffic, over ANY port, from ANY destination.

Verified the machine was reachable from the public internet via a simple ping command.



Step 3: Observe Live Attacks
Within minutes of turning off the firewalls, malicious actors and automated scanners discovered the machine.

verfied through the log files.


Step 4: Configure Centralized Logging
Created a Log Analytics Workspace (LAW) in Azure.

Deployed Microsoft Sentinel and linked it to the newly created Log Analytics Workspace.

Navigated to Sentinel's Content Hub and installed the Syslog Event connector.

Configured a Data Collection Rule (DCR) using the Azure Monitor Agent to automatically forward all local security logs from the VM into the Log Analytics Workspace.



Step 5: Map the Attackers
Logs alone only show the attacker's IP address. To visualize where they are coming from:

Created a custom Watchlist in Sentinel by uploading a CSV/JSON mapping file containing GeoIP data (matching IP blocks to Cities, Countries, Latitude, and Longitude).

Wrote a KQL query in Sentinel to pull Event ID 4625, extract the attacker's IP, and join it with the custom Watchlist to derive their physical coordinates.

Created a Sentinel Workbook to visualize the ingested geographic data onto a global Heat Map.


💡 Results & Takeaways
Speed of Discovery: It is staggering to observe how quickly a public-facing, vulnerable machine is discovered and brute-forced by automated malicious scanners (often within 10-15 minutes).

SIEM Basics: Successfully ingested raw logs from a standalone endpoint, utilized Kusto Query Language (KQL) to parse out the noise, and enriched the data using custom watchlists.

Visualization: Built an automated process to map uncontextualized raw data (IP addresses) into a dynamic, visual format (Global Threat Map).
