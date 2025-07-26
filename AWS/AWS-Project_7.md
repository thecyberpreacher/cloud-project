---
CyberPreacher Lab:
  title: '07 - Building a Cloud-Based SOC Lab on AWS'
  difficulty: 'Medium'
---

# Project 07: Building a Cloud-Based SOC Lab  
## CyberPreacher Edition  

## Project scenario  

In this project, you will build a simulated SOC (Security Operations Center) environment in AWS, deliberately leaving an EC2 instance vulnerable to observe real-world attack traffic. Logs will be ingested into Amazon CloudWatch and analyzed using Amazon Athena, including geolocation mapping of attacker IPs.

> All resources will be created in a region close to your location. Ensure consistency across your AWS account, VPC, and region.

## Lab objectives  

In this lab, you will complete the following exercises:  
- Exercise 1: Create a VPC and Subnet  
- Exercise 2: Launch and Configure an EC2 Instance  
- Exercise 3: Configure Security Groups and Disable Firewall  
- Exercise 4: Simulate Attacks and Analyze Logs  
- Exercise 5: Set Up CloudWatch Logs and Query with Visualization

## Instructions  

### Exercise 1: Create a VPC and Subnet  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Create a VPC named **VPC-SOCLab**  
- Task 2: Create a public subnet named **Subnet-SOCLab**  

#### Task 1: Create a VPC  

1. Open the [AWS Management Console](https://aws.amazon.com/console/) and log in.  
2. Navigate to the **VPC Dashboard**.  
3. Click **Create VPC** and configure the following:  
  - **Name tag**: Enter `VPC-SOCLab`.  
  - **IPv4 CIDR block**: Enter `192.168.0.0/16`.  
  - Leave other settings as default.  
4. Click **Create VPC**.  

#### Task 2: Create a Public Subnet  

1. In the **VPC Dashboard**, select **Subnets** and click **Create subnet**.  
2. Configure the following:  
  - **Name tag**: Enter `Subnet-SOCLab`.  
  - **VPC ID**: Select `VPC-SOCLab`.  
  - **Availability Zone**: Select a zone in your region (e.g., `us-east-1a`).  
  - **IPv4 CIDR block**: Enter `192.168.10.0/24`.  
3. Click **Create subnet**.  

4. Enable auto-assign public IP for the subnet:  
  - Select the subnet and click **Actions** > **Edit Subnet Settings**.  
  - Enable **Auto-assign public IPv4 address** and save changes.  

> Result: A VPC and public subnet are created successfully.

### Exercise 2: Launch and Configure an EC2 Instance  
#### Estimated timing: 15 minutes  

In this exercise, you will:  
- Task 1: Launch an EC2 instance named **TI-NET-EAST-1**  
- Task 2: Assign CloudWatch IAM role to new Instance.

#### Task 1: Launch an EC2 Instance  

1. Open the [AWS Management Console](https://aws.amazon.com/console/) and navigate to the **EC2 Dashboard**.  
2. Click **Launch Instances** and configure the following:  
  - **Name**: Enter `TI-NET-EAST-1`.  
  - **AMI**: Select **Microsoft Windows Server 2019 Base**.  
  - **Instance type**: Select **t2.micro** (eligible for free tier).  
  - **Key pair**: Create or select an existing key pair.  
  - **Network settings**:  
    - **VPC**: Select `VPC-SOCLab`.  
    - **Subnet**: Select `Subnet-SOCLab-Public`.  (Ensure you select the public subnet)
    - **Auto-assign public IP**: Disabled.  
  - **Security group**: Create a new security group with the following rule:  
    - **Type**: RDP  
    - **Source**: Anywhere (0.0.0.0/0).  
3. Click **Launch Instance**.  

4. Once the instance is running, note the public IP address.  

> Result: A Windows Server EC2 instance is deployed successfully.

#### Task 2: Assign CloudWatch IAM Role to New Instance  

1. **Create an IAM Role**:  
  - Open the [IAM Dashboard](https://aws.amazon.com/console/) in the AWS Management Console.  
  - Navigate to **Roles** and click **Create role**.  
  - Select **AWS service** as the trusted entity type.  
  - Choose **EC2** as the use case and click **Next**.  

2. **Attach Permissions**:  
  - Search for and select the policy **CloudWatchAgentServerPolicy**.  
  - Click **Next** and provide a name for the role, e.g., `CloudWatchAgentRole`.  
  - Review the settings and click **Create role**.  

3. **Assign the Role to the EC2 Instance**:  
  - Navigate to the **EC2 Dashboard** and select the instance `TI-NET-EAST-1`.  
  - Click **Actions** > **Security** > **Modify IAM role**.  
  - Select the role `CloudWatchAgentRole` from the dropdown and click **Update IAM role**.  

> Result: The EC2 instance is now configured with the IAM role required for CloudWatch integration.

### Exercise 3: Configure Security Groups and Disable Firewall  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Modify the security group to allow all inbound traffic  
- Task 2: Disable the Windows Firewall  

#### Task 1: Modify the Security Group  

1. In the **EC2 Dashboard**, select **Security Groups**.  
2. Locate the security group associated with `TI-NET-EAST-1`.  
3. Edit the inbound rules and add the following rule:  
  - **Type**: All traffic  
  - **Source**: Anywhere (0.0.0.0/0).  
4. Save the changes.  

#### Task 2: Disable the Windows Firewall  

1. Connect to the EC2 instance using RDP.  
2. Log in with the credentials you set during instance creation.  
3. Open the **Run** dialog (`Win + R`), type `wf.msc`, and press **Enter**.  
4. In the **Windows Defender Firewall with Advanced Security** window:  
  - For each profile (Domain, Private, and Public), set **Firewall state** to **Off**.  
5. Save the changes and close the window.  

> Result: The EC2 instance is fully exposed to incoming traffic.

### Exercise 4: Simulate Attacks and Analyze Logs  
#### Estimated timing: 10 minutes  

In this exercise, you will:  
- Task 1: Simulate failed login attempts  
- Task 2: Analyze logs in the Windows Event Viewer  

#### Task 1: Simulate Failed Login Attempts  

1. Use an RDP client to attempt logging into the EC2 instance with incorrect credentials.  
2. Repeat the process multiple times to generate failed login events.  

#### Task 2: Analyze Logs  

1. Log in to the EC2 instance using the correct credentials.  
2. Open the **Event Viewer** and navigate to:  
  - **Windows Logs** > **Security**  
3. Filter logs by **Event ID 4625** to view failed login attempts.  

> Result: Failed login attempts are logged in the EC2 instance.

### Exercise 5: Set Up CloudWatch Logs and Query with Visualization
#### Estimated timing: 20 minutes  

In this exercise, you will:  
- Task 1: Create a CloudWatch log group  
- Task 2: Install the CloudWatch agent on the EC2 instance  
- Task 3: Query logs using Amazon Athena  

#### Task 1: Create a CloudWatch Log Group  

1. Open the **CloudWatch Dashboard** and navigate to **Log groups**.  
2. Click **Create log group** and name it `SOC-Logs`.  
3. Open the new log group **SOC-Logs**, and create a new log stream **windows**.

#### Task 2: Install the CloudWatch Agent  

1. Download and install the CloudWatch agent on the EC2 instance.  
2. Configure the agent to send Windows Event Logs to the `SOC-Logs` log group.  

1. **Download the CloudWatch Agent**:  
  - Connect to the EC2 instance using RDP.  
  - Open a web browser and download the CloudWatch agent from the [AWS CloudWatch Agent Downloads](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html).  
  - Select the appropriate version for Windows and save the installer.  

2. **Install the CloudWatch Agent**:  
  - Run the downloaded installer and follow the on-screen instructions to complete the installation.  

3. **Configure the CloudWatch Agent**:  
  - Open a command prompt as an administrator.  
  - Navigate to the CloudWatch agent installation directory (e.g., `C:\Program Files\Amazon\AmazonCloudWatchAgent`).  
  - Run the following command to create a default configuration file:  
    ```cmd
    amazon-cloudwatch-agent-config-wizard
    ```  
    |settings|value|
    |Log group name|SOC-Logs|
    |Log stream name|windows|
    > Accept all other defaults values except **Uploading Config to SSM**.

  - Follow the prompts to configure the agent to send Windows Event Logs to CloudWatch.  

4. **Start the CloudWatch Agent**:  
  - Run the following command to start the agent:  
    ```cmd
    amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:'C:\Program Files\Amazon\AmazonCloudWatchAgent\config.json'
    ```  

5. **Verify Log Ingestion**:  
  - Open the **CloudWatch Dashboard** in the AWS Management Console.  
  - Navigate to **Log groups** and confirm that logs are being sent to the `SOC-Logs` log group.  

> Result: The CloudWatch agent is installed and configured to send logs to CloudWatch.
#### Task 3: Query Logs Using Logs Insight  

1. Open the **Logs Insights** and set up a query table for the CloudWatch logs.  
2. Run a query to analyze failed login attempts.  
3. Visualize the output of the failed login by IP address.

#### Task 3: Query Logs Using Logs Insight  

1. **Open Logs Insights**:  
  - Navigate to the **CloudWatch Dashboard** in the AWS Management Console.  
  - Select **Log groups** and click on `SOC-Logs`.  
  - Click **View in Logs Insights**.  

2. **Set Up a Query**:  
  - In the query editor, enter the following query to filter failed login attempts:  
    ```sql
    fields @timestamp, @message
    | filter @message like /4625/
    | sort @timestamp desc
    | limit 20
    ```  
  - Click **Run query** to execute the query.  

3. **Analyze Results**:  
  - Review the query results to identify failed login attempts.  
  - Note the IP addresses and timestamps associated with the events.  

4. **Visualize Data**:  
  - Modify the query to group failed login attempts by IP address:  
    ```sql
    fields @timestamp, @message
    | filter @message like /4625/
    | parse @message /Source Network Address:\s+(?<sourceIP>\S+)/
    | stats count(*) as failedLogins by sourceIP, bin(1h)
    | sort failedLogins desc

    ```  
  - Click **Run query** to generate a visualization.  
    ```sql
    fields @timestamp, @message
    | filter @message like /4625/
    | parse @message /Source Network Address:\s+(?<sourceIP>\S+)/
    | stats count(*) as failedLogins by sourceIP, bin(1h)
    | sort failedLogins desc
    ```
5. **Export Insights**:  
  - Save the query results or export them for further analysis.  
  - Optionally, create a dashboard in CloudWatch to monitor failed login attempts over time.  

> Result: Logs are queried and visualized to identify patterns in failed login attempts.

## Summary of Key Security Practices  

- **Visibility**: Monitoring failed login attempts via Event ID  
- **Threat Intelligence**: Mapping attacker IPs to real-world locations  
- **Logging**: Logs are ingested into CloudWatch for analysis  
- **Awareness**: Open exposure attracts real adversaries and illustrates global attack patterns  

> Note: Delete all resources once the project is completed to prevent unnecessary charges.

> Side Task: Share screenshots on LinkedIn showing attack visualization and include hashtags **#cloudprojectwithcyberpreacher** and **#CPwCP**. Share your experience and learnings from this SOC lab.
