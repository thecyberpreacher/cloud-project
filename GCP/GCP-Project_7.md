---
CyberPreacher Lab:
   title: '07 - Building a Cloud-Based SOC Lab on GCP'
   difficulty: 'Medium'
---

# Project 07: Building a Cloud-Based SOC Lab  
## CyberPreacher Edition  

## Project scenario  

In this project, you will build a simulated SOC (Security Operations Center) environment in GCP, deliberately leaving a VM instance vulnerable to observe real-world attack traffic. Logs will be ingested into Cloud Logging and analyzed using BigQuery, including geolocation mapping of attacker IPs.

> All resources will be created in a region close to your location. Ensure consistency across your GCP project, network, and region.

## Lab objectives  

In this lab, you will complete the following exercises:  
- Exercise 1: Create a VPC and Subnet  
- Exercise 2: Launch and Configure a VM Instance  
- Exercise 3: Configure Firewall Rules  
- Exercise 4: Simulate Attacks and Analyze Logs  
- Exercise 5: Set Up Cloud Logging and Query with Visualization  

## Instructions  

### Exercise 1: Create a VPC and Subnet  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Create a VPC named **VPC-SOCLab**  
- Task 2: Create a public subnet named **Subnet-SOCLab**  

#### Task 1: Create a VPC  

1. Open the [Google Cloud Console](https://console.cloud.google.com/) and log in.  
2. Navigate to **VPC Network** > **Create VPC network**.  
3. Configure the following:  
   - **Name**: Enter `VPC-SOCLab`.  
   - **Subnet mode**: Select **Custom**.  
4. Click **Create**.  

#### Task 2: Create a Public Subnet  

1. In the **VPC Network** section, select **Subnets** and click **Create subnet**.  
2. Configure the following:  
   - **Name**: Enter `Subnet-SOCLab`.  
   - **Region**: Select a region close to your location (e.g., `us-central1`).  
   - **IP address range**: Enter `192.168.10.0/24`.  
3. Click **Create**.  

4. Enable external IP addresses for the subnet:  
   - Select the subnet and click **Edit**.  
   - Ensure **Private Google Access** is enabled.  

> Result: A VPC and public subnet are created successfully.

### Exercise 2: Launch and Configure a VM Instance  
#### Estimated timing: 15 minutes  

In this exercise, you will:  
- Task 1: Launch a VM instance named **TI-NET-CENTRAL-1**  
- Task 2: Assign Cloud Logging IAM role to the instance.

#### Task 1: Launch a VM Instance  

1. Open the [Google Cloud Console](https://console.cloud.google.com/) and navigate to **Compute Engine** > **VM instances**.  
2. Click **Create Instance** and configure the following:  
   - **Name**: Enter `TI-NET-CENTRAL-1`.  
   - **Region**: Select `us-central1`.  
   - **Zone**: Select `us-central1-a`.  
   - **Machine type**: Select **e2-micro** (eligible for free tier).  
   - **Boot disk**: Select **Windows Server 2019**.  
   - **Network interfaces**:  
      - **Network**: Select `VPC-SOCLab`.  
      - **Subnet**: Select `Subnet-SOCLab`.  
      - **External IP**: Select **Ephemeral**.  
3. Click **Create**.  

4. Once the instance is running, note the external IP address.  

> Result: A Windows Server VM instance is deployed successfully.

#### Task 2: Assign Cloud Logging IAM Role to the Instance  

1. **Create an IAM Role**:  
   - Open the **IAM & Admin** section in the Google Cloud Console.  
   - Navigate to **Roles** and click **Create Role**.  
   - Provide a name for the role, e.g., `CloudLoggingAgentRole`.  
   - Add the permission **roles/logging.logWriter**.  
   - Save the role.  

2. **Assign the Role to the VM Instance**:  
   - Navigate to **Compute Engine** > **VM instances**.  
   - Select the instance `TI-NET-CENTRAL-1`.  
   - Click **Edit** and under **Service account**, select the service account with the `CloudLoggingAgentRole` role.  
   - Save changes.  

> Result: The VM instance is now configured with the IAM role required for Cloud Logging integration.

### Exercise 3: Configure Firewall Rules  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Allow all inbound traffic  
- Task 2: Disable the Windows Firewall  

#### Task 1: Configure Firewall Rules  

1. Navigate to **VPC Network** > **Firewall rules**.  
2. Click **Create Firewall Rule** and configure the following:  
   - **Name**: Enter `Allow-All-Inbound`.  
   - **Network**: Select `VPC-SOCLab`.  
   - **Direction**: Select **Ingress**.  
   - **Action**: Select **Allow**.  
   - **Targets**: Select **All instances in the network**.  
   - **Source IP ranges**: Enter `0.0.0.0/0`.  
   - **Protocols and ports**: Select **Allow all**.  
3. Click **Create**.  

#### Task 2: Disable the Windows Firewall  

1. Connect to the VM instance using RDP.  
2. Log in with the credentials you set during instance creation.  
3. Open the **Run** dialog (`Win + R`), type `wf.msc`, and press **Enter**.  
4. In the **Windows Defender Firewall with Advanced Security** window:  
   - For each profile (Domain, Private, and Public), set **Firewall state** to **Off**.  
5. Save the changes and close the window.  

> Result: The VM instance is fully exposed to incoming traffic.

### Exercise 4: Simulate Attacks and Analyze Logs  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Simulate failed login attempts  
- Task 2: Analyze logs in the Windows Event Viewer  

#### Task 1: Simulate Failed Login Attempts  

1. Use an RDP client to attempt logging into the VM instance with incorrect credentials.  
2. Repeat the process multiple times to generate failed login events.  

#### Task 2: Analyze Logs  

1. Log in to the VM instance using the correct credentials.  
2. Open the **Event Viewer** and navigate to:  
   - **Windows Logs** > **Security**  
3. Filter logs by **Event ID 4625** to view failed login attempts.  

> Result: Failed login attempts are logged in the VM instance.

### Exercise 5: Set Up Cloud Logging and Query with Visualization  
#### Estimated timing: 20 minutes  

In this exercise, you will:  
- Task 1: Enable Cloud Logging  
- Task 2: Query logs using BigQuery  

#### Task 1: Enable Cloud Logging  

1. Navigate to **Operations** > **Logging** > **Logs Explorer**.  
2. Confirm that logs from the VM instance are being ingested into Cloud Logging.  

#### Task 2: Query Logs Using BigQuery  

1. Export logs from Cloud Logging to BigQuery:  
   - Navigate to **Logging** > **Log Router**.  
   - Create a sink to export logs to a BigQuery dataset.  

2. Analyze logs in BigQuery:  
   - Open the BigQuery Console and select the dataset containing the exported logs.  
   - Run a query to filter failed login attempts:  
      ```sql
      SELECT
         timestamp,
         jsonPayload.message,
         jsonPayload.sourceIP
      FROM
         `project_id.dataset_id.table_id`
      WHERE
         jsonPayload.message LIKE '%4625%'
      ORDER BY
         timestamp DESC
      LIMIT
         20;
      ```  

3. Visualize data using Data Studio or BigQuery charts.  

> Result: Logs are queried and visualized to identify patterns in failed login attempts.

## Summary of Key Security Practices  

- **Visibility**: Monitoring failed login attempts via Event ID  
- **Threat Intelligence**: Mapping attacker IPs to real-world locations  
- **Logging**: Logs are ingested into Cloud Logging for analysis  
- **Awareness**: Open exposure attracts real adversaries and illustrates global attack patterns  

> Note: Delete all resources once the project is completed to prevent unnecessary charges.

> Side Task: Share screenshots on LinkedIn showing attack visualization and include hashtags **#cloudprojectwithcyberpreacher** and **#CPwCP**. Share your experience and learnings from this SOC lab.

