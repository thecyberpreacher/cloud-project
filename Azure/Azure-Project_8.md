---
CyberPreacher Lab:
    title: '08 - Case Study 1'
    difficulty: 'Hard'
---

# Project 08: Case Study 1
## CyberPreacher Edition  

## Summary

### Compamy Overview  

CyberPreacher LTD is a digital media company that has 5 employees in the Chicago area and 10 employees in the San Francisco area.

Existing Environment

CyberPreeacher LTD has an Azure subscription named Sub1 that has a Subscription ID of XXXXXXXX-XXXX-XXXXXXXX-XXXXXXXXXXXX

Sub1 is associated to an Azure Entra ID tenant named example.com. The tenant contains the user objects and the device objects of all the CyberPreacher employees and their devices. Each user is assigned an Azure entra ID Premium P2 License. Entra ID Privileged Identity Management (PIM) is activated.

The Tenant contains the groups shown in the following table.

|Name|Type|Description|
|---|---|---|
|Group1|Security group|A group that has the Dynamic user membership type, contains all the San Francisco users, and provides access to many Azure AD applications and Azure resources.|
|Group2|Security group|A group that has the Dynamic user membership type and contains the Chicago It team.|

The Azure Subscription contains the following Resource Groups. **AZ500RG1** and **AZ500RG2**

**AZ500RG1** has the following resources:
![AZ500RG1 Resource Group Overview](media/AZ500RG1.jpg)

To reconstruct the environment, use the prebuilt ARM Deployment Template that I created for this Lab.

1. **Resource Group 1**: [AZ500RG1](../Lab_Files/AZ500RG1.zip.zip) 
2. **SQL Server**: [SQL-DB](../Lab_Files/SQL_AZ500.zip.zip)
3. Manually create an empty Resource Group 2 named AZ500RG2.
4. Bulk import Users: [Users](../Lab_Files/bulkusertemplate.csv)
4. Create **Group1** Security group in Entra ID, ensuring that it uses **Dynamic User Membership** based users in San Francisco.
5. Create **Group2** Security group in Entra ID, ensuring that it uses **Dynamic User Membership** based on users in Chicago.

AZ500RG2 is a resource group that contains shared IT resources.

> All resources will be created in a East US 2.

#### Planned Changes  

|Name|Type|Description|
|---|---|---|
|Firewall|Azure Firewall|An Azure Fierwall on VNET1|
|RT1|Route Table|A route table that will contain a route pointing to Firewall1 as the default gateway and will be assigned to Subnet0|
|AKS1|Azure Kubernetes Services(AKS)|A managed AKS cluster|

- All San Francisco users and their devices must be members of Group 1.
- The members of group 2 must be assigned the contributor role of **AZ500RG2** by using a permanent eligibility assignment.
- Users must be prevented from registering applications in the Entra ID and from consenting to applications that access company information on the users' behalf.

#### Identity and Access Requirement

- All San Francisco users and their devices must be members of Group1

- The members of Group2 must be assigned the contributor role to Resource group2 by using permanent eligible assignment.

- Users must be prevented from registering applications in Azure AD and from consenting to applications that access company information on the users' behalf.

#### Platform Protection Requirements

CyberPreacher LTD identifies the following platform protection protection requirements:

- Microsoft Antimalware must be installed on the Virtual machines in **AZ500RG1**.

- Members of Group2 must be assigned the Azure Kubernetes Service Cluster Admin Role.

- Entra ID users must be able to authenticate to AKS1 by using their Entra ID credentials.

- Following the implementation of the planned changes, the IT team must be able to connect to VM0 by using JIT VM access.

- A new custom RBAC role name Role1 must be used to delegate the administration of the managed disks in **AZ500RG1**. Role1 must be available only for RG1

## Solution  

### Meeting the Planned Changes requirements
### Exercise 1: Deploy an Azure Firewall on VNET1  
#### Estimated timing: 10 minutes  

#### Step-by-Step: Deploy Azure Firewall on VNET1

1. **Navigate to the Azure Portal**
    - Go to [https://portal.azure.com](https://portal.azure.com) and sign in with your credentials.

2. **Open the Resource Group**
    - In the left menu, select **Resource groups**.
    - Click on **AZ500RG1**.

3. **Create Azure Firewall**
    - Click **Create** > **Resource**.
    - Search for **Firewall** and select **Azure Firewall**.
    - Click **Create**.

4. **Configure Firewall Basics**
    - **Subscription**: Select *your subscription*.
    - **Resource Group**: Choose *AZ500RG1*.
    - **Name**: Enter a name (e.g., `Firewall1`).
    - **Region**: Select *East US 2*.

5. **Configure Firewall Settings**
    - **Firewall management**: Choose *Use Firewall Policy* (Create New Basic Policy).
    - **Virtual network**: Select *VNET1*.
    - **Public IP address**: Click *Create new*, provide a name (e.g., `Firewall1-pip`), and click *OK*.

6. **Review and Create**
    - Click **Review + create**.
    - After validation, click **Create**.

7. **Verify Deployment**
    - Once deployment completes, go to **AZ500RG1** and confirm that **Firewall1** and its public IP are present.

8. **(Optional) Configure Firewall Rules**
    - Open **Firewall1**.
    - Add network or application rules as needed for your environment.


### Exercise 2: Create Route Table
#### Description: A route table that will contain a route pointing to Firewall1 as the default gateway and will be assigned to Subnet0

1. **Navigate to the Azure Portal**
    - Go to [https://portal.azure.com](https://portal.azure.com) and sign in.

2. **Open the Resource Group**
    - In the left menu, select **Resource groups**.
    - Click on **AZ500RG1**.

3. **Create a Route Table**
    - Click **Create** > **Resource**.
    - Search for **Route Table** and select it.
    - Click **Create**.

4. **Configure Route Table Basics**
    - **Subscription**: Select your subscription.
    - **Resource Group**: Choose *AZ500RG1*.
    - **Region**: Select *East US 2*.
    - **Name**: Enter a name (e.g., `RT1`).
    - Click **Review + create**, then **Create**.

5. **Add a Route to the Route Table**
    - After deployment, open the new route table (**RT1**).
    - In the left menu, select **Routes** > **Add**.
    - **Route name**: Enter a name (e.g., `DefaultToFirewall`).
    - **Address prefix destination**: Enter `0.0.0.0/0`.
    - **Next hop type**: Select **Virtual appliance**.
    - **Next hop address**: Enter the private IP address of **Firewall1**.
    - Click **Add**.

6. **Associate Route Table with Subnet0**
    - In the route table (**RT1**), select **Subnets** > **Associate**.
    - Select **VNET1** and then choose **Subnet0**.
    - Click **OK** to associate.

7. **Verify Configuration**
    - Confirm that **RT1** is associated with **Subnet0** and the route points to **Firewall1** as the default gateway.

### Exercise 3: Deploying Azure Kubernetes Service (AKS)
#### Description: A managed AKS cluster

