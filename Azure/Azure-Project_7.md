---
CyberPreacher Lab:
    title: '07 - Building a Cloud-Based SOC Lab'
    difficulty: 'Medium'
---

# Project 07: Building a Cloud-Based SOC Lab  
## CyberPreacher Edition  

## Project scenario  

In this project, you will build a simulated SOC (Security Operations Center) environment in Azure, deliberately leaving a virtual machine vulnerable to observe real-world attack traffic. Logs will be ingested into Microsoft Sentinel for analysis, including geolocation mapping of attacker IPs.

> All resources will be created in a region close to your location. Ensure consistency across subscription, resource group, and region.

## Lab objectives  

In this lab, you will complete the following exercises:  
- Exercise 1: Create Resource Group and Virtual Network  
- Exercise 2: Deploy and Configure Virtual Machine  
- Exercise 3: Configure Network Security Rules and Disable Firewall  
- Exercise 4: Simulate Attacks and Analyze Logs  
- Exercise 5: Set Up Microsoft Sentinel and Ingest Security Logs  
- Exercise 6: Upload Geolocation Watchlist and Visualize Attacks  

## Instructions  

### Exercise 1: Create Resource Group and Virtual Network  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Create a resource group named **RG-SOCLab**  
- Task 2: Create a virtual network named **VN-SOCLab** in the same region  

#### Create a Resource Group  

1. Open [Azure Portal](https://portal.azure.com) and log in with your credentials.  

2. In the left-hand menu, select **Resource groups**.  

3. Click **+ Create** at the top of the Resource Groups page.  

4. Configure the following settings:  
    - **Subscription**: Select your subscription.  
    - **Resource group**: Enter a name for your resource group (e.g., `RG-SOCLab`).  
    - **Region**: Select a region close to your location (e.g., `East US`).  

5. Click **Review + Create**.  

6. Validate the configuration and click **Create**.  

#### Task 2: Create a Virtual Network  

1. Open [Azure Portal](https://portal.azure.com) and log in with your credentials.  

2. In the left-hand menu, select **Virtual networks**.  

3. Click **+ Create** at the top of the Virtual Networks page.  

4. Configure the following settings under the **Basics** tab:  
    - **Subscription**: Select your subscription.  
    - **Resource group**: Select **RG-SOCLab**.  
    - **Name**: Enter **VN-SOCLab**.  
    - **Region**: Select the same region as your resource group.  

5. Navigate to the **IP Addresses** tab:  
    - **IPv4 address space**: Enter `192.168.0.0/16`.  
    - **Subnets**: Click **+ Add subnet** and configure the following:  
        - **Subnet name**: Enter **default**.  
        - **Subnet address range**: Enter `192.168.10.0/24`.  
    - Click **Add** to save the subnet.  

6. Leave the default settings for **Security**, **Tags**, and **Review + Create**.  

7. Click **Review + Create**.  

8. Validate the configuration and click **Create**.  

### Exercise 2: Deploy and Configure Virtual Machine  
#### Estimated timing: 15 minutes  

In this exercise, you will:  
Task 1: Create a virtual machine named **TI-NET-EAST-1**  

1. Open [Azure Portal](https://portal.azure.com) and log in with your credentials.  

2. In the left-hand menu, select **Virtual Machines** and click **+ Create** > **Azure Virtual Machine**.  

3. Configure Basic Settings  
    - **Subscription**: Select your subscription.  
    - Resource Group**: Select **RG-SOCLab** (or create it if not already created).  
    - **Virtual Machine Name**: Enter **TI-NET-EAST-1**.  
    - **Region**: Select the same region as your resource group.  
    - **Image**: Select **Windows 10 Pro, version 22H2 - x64 Gen2**.  
    - **Size**: Click **Change size** and select **Standard_B1s**.  
    - **Administrator Account**:  
    - Username: Enter a username (e.g., `adminuser`).  
    - Password: Enter a strong password and confirm it.  

4. Leave the default settings for the OS disk.  

5. Configure Networking  
    - **Virtual Network**: Select **VN-SOCLab**.  
    - **Subnet**: Use the default or select a specific subnet.  
    - **Public IP**: Ensure a public IP is assigned.  
    - **NIC Network Security Group**: Select **Basic**.  

6. Enable Auto-Delete for Public IP and NIC  
    - Under the **Networking** tab, locate the **Public IP** section.  
    - Enable **Delete public IP when VM is deleted**.  
    - Under the **Advanced** tab, enable **Delete NIC when VM is deleted**.  

7. Disable Boot Diagnostics  
    - Navigate to the **Management** tab.  
    - Set **Boot diagnostics** to **Off**.  

8. Review and Create  
    - Click **Review + Create**.  
    - Validate the configuration and click **Create**.  

9. Access the Virtual Machine  
1. Once deployment is complete, navigate to the **Virtual Machines** section.  
2. Select **TI-NET-EAST-1** and note the public IP address.  
3. Use an RDP client to connect to the VM using the credentials you set earlier.  

> Result: A Windows 10 22H2 virtual machine with the specified configuration is deployed successfully. The VM will be added to the resource group. Additional resources like NSG will be created automatically.

### Exercise 3: Configure Network Security Rules and Disable Firewall  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Delete default RDP inbound rule in NSG  
- Task 2: Create a custom inbound rule with the following configuration:  
- Task 3: Connect to VM using RDP and disable firewall via **wf.msc**

#### Task 1: Delete Default RDP Inbound Rule in NSG  

1. Open [Azure Portal](https://portal.azure.com) and log in with your credentials.  
2. In the left-hand menu, select **Resource groups** and navigate to **RG-SOCLab**.  
3. Locate the **Network Security Group (NSG)** associated with **TI-NET-EAST-1**.  
4. Click on the NSG and navigate to the **Inbound security rules** tab.  
5. Identify the default RDP rule (usually named something like `AllowRDP`).  
6. Select the rule and click **Delete**.  
7. Confirm the deletion when prompted.  


#### Task 2: Create a Custom Inbound Rule  

1. In the same **Inbound security rules** tab of the NSG, click **+ Add** to create a new rule.  
2. Configure the rule with the following settings:  
    - **Source**: Any  
    - **Source port ranges**: *  
    - **Destination**: Any  
    - **Destination port ranges**: *  
    - **Protocol**: Any  
    - **Action**: Allow  
    - **Priority**: 100  
    - **Name**: `DANGER_AllowAnyCustomAnyInbound`  
3. Click **Add** to save the rule.  

#### Task 3: Connect to VM Using RDP and Disable Firewall via **wf.msc**

1. Open an RDP client on your local machine.  
2. Enter the public IP address of the virtual machine **TI-NET-EAST-1**.  
3. Use the credentials you set during the VM creation to log in.  
4. Once logged in, press **Win + R** to open the Run dialog box.  
5. Type `wf.msc` and press **Enter** to open the Windows Defender Firewall with Advanced Security.  
6. In the left-hand menu, select **Windows Defender Firewall Properties**.  
7. For each profile (Domain, Private, and Public):  
    - Click on the profile tab.  
    - Set **Firewall state** to **Off**.  
    - Click **OK** to save changes.  
8. Close the firewall settings window.  

> Result: The firewall is disabled, leaving the VM fully exposed to incoming traffic. VM becomes vulnerable to public attacks.


### Exercise 4: Simulate Attacks and Analyze Logs  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Simulate failed login attempts  
- Task 2: Open Event Viewer and filter logs by **Event ID 4625**  
  Identify failed login attempts and attacker usernames  

#### Task 1: Simulate Failed Login Attempts  

1. Open an RDP client on your local machine.  
2. Enter the public IP address of the virtual machine **TI-NET-EAST-1**.  
3. Use incorrect credentials to attempt logging into the VM multiple times.  
    - Example: Use a random username (e.g., `hacker01`) and a random password.  
4. Repeat the process at least 5–10 times to generate failed login events.  

> Result: Failed login attempts will be logged in the VM's security logs.  

#### Task 2: Open Event Viewer and Filter Logs by Event ID 4625  

1. Log in to the virtual machine **TI-NET-EAST-1** using the correct credentials via RDP.  
2. Open the **Start Menu** and search for **Event Viewer**.  
3. In the Event Viewer, navigate to:  
    - **Windows Logs** > **Security**  
4. In the right-hand menu, click **Filter Current Log**.  
5. In the filter dialog box:  
    - Under **Event IDs**, enter `4625`.  
    - Click **OK** to apply the filter.  
6. Review the filtered logs to identify failed login attempts.  
    - Look for details such as the attacker’s username and source IP address.  

> Result: You will see a list of failed login attempts with relevant details.  

### Exercise 5: Set Up Microsoft Sentinel and Ingest Security Logs  
#### Estimated timing: 20 minutes  

In this exercise, you will:  
- Task 1: Create Log Analytics Workspace  
- Task 2: Install "Windows Security Events via AMA" from Sentinel Content Hub  
- Task 3: Create a Data Collection Rule named **DCR-Windows**  
  Link to VM and begin collecting logs  
- Task 4: Run basic KQL query in Sentinel Logs pane:  
  `SecurityEvent`

#### Task 1: Create Log Analytics Workspace  

1. Open [Azure Portal](https://portal.azure.com) and log in with your credentials.  
2. In the left-hand menu, search for **Log Analytics Workspaces** and select it.  
3. Click **+ Create** at the top of the page.  
4. Configure the following settings:  
    - **Subscription**: Select your subscription.  
    - **Resource group**: Select **RG-SOCLab**.  
    - **Name**: Enter a name for the workspace (e.g., `SOC-Logs`).  
    - **Region**: Select the same region as your resource group.  
5. Click **Review + Create**.  
6. Validate the configuration and click **Create**.  

#### Task 2: Install "Windows Security Events via AMA" from Sentinel Content Hub  

1. In the Azure Portal, search for **Microsoft Sentinel** and select it.  
2. Click **+ Add** to attach Sentinel to your Log Analytics Workspace.  
3. Select the workspace you created in Task 1 (e.g., `SOC-Logs`) and click **Add**.  
4. Once Sentinel is added, navigate to the **Content Hub** tab in Sentinel.  
5. Search for **Windows Security Events via AMA** in the Content Hub.  
6. Click on the item and select **Install**.  

#### Task 3: Create a Data Collection Rule (DCR)  

1. In the Azure Portal, search for **Data Collection Rules** and select it.  
2. Click **+ Create** at the top of the page.  
3. Configure the following settings:  
    - **Subscription**: Select your subscription.  
    - **Resource group**: Select **RG-SOCLab**.  
    - **Name**: Enter `DCR-Windows`.  
    - **Region**: Select the same region as your resource group.  
4. Under **Resources**, click **+ Add resources** and select the virtual machine **TI-NET-EAST-1**.  
5. Under **Data Sources**, click **+ Add data source** and select **Windows Security Events**.  
6. Click **Next**, review the configuration, and click **Create**.  

#### Task 4: Run Basic KQL Query in Sentinel Logs Pane  

1. Open [Azure Portal](https://portal.azure.com) and navigate to **Microsoft Sentinel**.  
2. Select the workspace you created in Task 1 (e.g., `SOC-Logs`).  
3. In the left-hand menu, select **Logs** under the **General** section.  
4. In the query editor, paste the following KQL query:  
    ```kql
    SecurityEvent
    ```  
5. Click **Run** to execute the query.  
6. Review the results to ensure logs are being ingested successfully.  

> Result: Logs from the virtual machine will begin populating in Sentinel after 30–40 minutes.  

### Exercise 6: Upload Geolocation Watchlist and Visualize Attacks  
#### Estimated timing: 20 minutes  

In this exercise, you will:  
- Task 1: Upload geolocation watchlist as a CSV named **geoip**  
  [From Josh Madakor’s YouTube description](https://youtu.be/g5JL2RIbThM?si=oJpcQnUF8DQ30BAD) 
- Task 2: Use enhanced KQL query to associate failed logins with geolocation:  
  ```
  let GeoIPDB_FULL = _GetWatchlist("geoip");  
  let WindowsEvents = SecurityEvent  
   | where EventID == 4625  
   | order by TimeGenerated desc  
   | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)  
   | summarize FailureCount = count() by IpAddress, latitude, longitude, cityname, countryname  
   | project FailureCount, AttackerIp = IpAddress, latitude, longitude, city = cityname, country = countryname,  
    friendly_location = strcat(cityname, " (", countryname, ")");
  ```

- Task 3: Paste the JSON snippet provided into the Advanced Editor of a Sentinel Workbook to visualize global attack traffic

> Result: Geospatial attack data appears in the map visualization  

---

## Summary of Key Security Practices  

- **Visibility**: Monitoring failed login attempts via Event ID  
- **Threat Intelligence**: Mapping attacker IPs to real-world locations  
- **Logging**: Logs are ingested via AMA into Sentinel for real-time analysis  
- **Awareness**: Open exposure attracts real adversaries and illustrates global attack patterns

> Note: Delete all resources once project is completed to prevent unnecessary charges.

> Side Task: Share screenshots on LinkedIn showing attack visualization and include hashtags **#cloudprojectwithcyberpreacher** and **#CPwCP**. Share your experience and learnings from this SOC lab.