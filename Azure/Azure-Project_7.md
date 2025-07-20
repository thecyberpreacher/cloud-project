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

---

## Instructions  

### Exercise 1: Create Resource Group and Virtual Network  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Create a resource group named **RG-SOCLab**  
- Task 2: Create a virtual network named **VN-SOCLab** in the same region  

### Exercise 2: Deploy and Configure Virtual Machine  
#### Estimated timing: 15 minutes  

In this exercise, you will:  
- Task 1: Create a virtual machine named **TI-NET-EAST-1**  
- Use **Windows 10 22H2** image  
- Select **Standard_B1s** VM size  
- Enable auto-delete for public IP and NIC  
- Disable boot diagnostics  
- Save login credentials for RDP access  

> Result: The VM will be added to the resource group. Additional resources like NSG will be created automatically.

---

### Exercise 3: Configure Network Security Rules and Disable Firewall  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Delete default RDP inbound rule in NSG  
- Task 2: Create a custom inbound rule with the following configuration:  
  Source: Any  
  Source port ranges: *  
  Destination: Any  
  Destination port ranges: *  
  Protocol: Any  
  Action: Allow  
  Priority: 100  
  Name: **DANGER_AllowAnyCustomAnyInbound**

- Task 3: Connect to VM using RDP and disable firewall via **wf.msc**

> Result: VM becomes vulnerable to public attacks.


### Exercise 4: Simulate Attacks and Analyze Logs  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Simulate failed login attempts  
- Task 2: Open Event Viewer and filter logs by **Event ID 4625**  
  Identify failed login attempts and attacker usernames  

> Result: Security logs will show real attack traffic.

---

### Exercise 5: Set Up Microsoft Sentinel and Ingest Security Logs  
#### Estimated timing: 20 minutes  

In this exercise, you will:  
- Task 1: Create Log Analytics Workspace  
- Task 2: Install "Windows Security Events via AMA" from Sentinel Content Hub  
- Task 3: Create a Data Collection Rule named **DCR-Windows**  
  Link to VM and begin collecting logs  
- Task 4: Run basic KQL query in Sentinel Logs pane:  
  `SecurityEvent`

> Result: Logs will begin populating after 30–40 minutes.

---

### Exercise 6: Upload Geolocation Watchlist and Visualize Attacks  
#### Estimated timing: 20 minutes  

In this exercise, you will:  
- Task 1: Upload geolocation watchlist as a CSV named **geoip**  
  (From Josh Madakor’s YouTube description)  
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

---

Let me know if you want help converting this into a guide, PDF, or post-ready article.