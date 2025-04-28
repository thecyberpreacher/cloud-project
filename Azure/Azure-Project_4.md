---
CyberPreacher Lab:
    title: '04 - Implement a Zero-Trust Architecture'
    difficulty: 'Medium'
---

# Project 04: Implement a Zero-Trust Architecture
## CyberPreacher Edition

## Project scenario

A mid-sized company is moving to a Zero-Trust security model in Azure. Currently, they rely on perimeter security but lack identity-based access controls, making them vulnerable to unauthorized access and insider threats. Their new Zero-Trust strategy focuses on:

- Verifying identities before granting access.
- Using Conditional Access policies to enforce security.
- Monitoring and auditing access attempts.
- Protecting workloads with network segmentation and access control policies.

> For all the resources in this lab, we are using the **East US** region.

### **Lab Objectives**
In this lab, you will complete the following exercises:

1. **Exercise 1:** Enable Entra ID Identity Protection and Conditional Access.
2. **Exercise 2:** Configure Multi-Factor Authentication (MFA) for users.
3. **Exercise 3:** Implement Network Segmentation and Private Access.
4. **Exercise 4:** Monitor and Audit Access Logs.

> Bulk import of users
### Task 1 - Bulk operations for creating users with a .csv file
1. In the Microsoft Entra ID menu, first open Identity, then select Users and then select All users.

2. On the **Users	All users** tile, select the Bulk operations drop-down arrow and then Bulk create.

3. Selecting Bulk create will open a new tile. This tile provides a Download link to a template file that you will edit to populate with your user information and upload to add the bulk creation of users.

4. Select Download to download the .csv file.

5. The .csv template provides you with the fields included with the user profile. This includes the required username, display name, and initial password. You can also complete optional fields, such as Department and Usage location, at this time. The following screenshot is an example of how you can complete the .csvfile:

- Bulk import using csv file entry

- You can modify this file to add users in bulk. Note that you do not need to fill out all the field. As per the sample data provide, you mainly need to add the name and username information.

6. A sample CSV has been provided in the Lab_Files folder – [SC300BulkUser.csv](../Lab_Files/SC300BulkUser.csv).
Open Notepad.
Inside the lab environment, select the START button and type Notepad.
Open the SC300BulkUser.csv file
Change the enter your domain name to the domain of your Azure lab environment.
Save the file.

7. On the Bulk create users dialog, select the file folder icon on step 3.

8. Path to the Allfiles/Labs/Lab1 folder and select SC300BulkUser.csv file.

9. Select Open.

10. You will be notified that the file uploaded successfully.  Choose Submit to add the users.

11. On the left pane, select **groups** and create new group.

12. Name the group **Management**, and add the previously added users to this group and create the group.

> After the users have been created, you will be prompted that the creation has succeeded. Close the Bulk create users tile and the new users will be populated in the list of **Users**

## Instructions
### **Exercise 1: Configure Conditional Access Policies in Entra ID**  
#### **Estimated Timing: 15 minutes**  

#### **Task 1: Create Conditional Access Policies**  
1. Navigate to **Azure portal**.  

2. In the search bar at the top of the portal, type **"Entra ID"** and select it from the search results.

2. Under **Manage** select **Security**.

3. Under **Protect**, select **Conditional Access**.

3. Click **+ Create New Policy**, name it **Zero-Trust Access Control**.  

4. Configure the following settings:  
   - **Users**: Select "Group", named **Management**.
   - *skip target resources and networks.*
   - **Conditions**:  
     - Configure **Insider risks**, to include **elevated** and **moderate**.  
     - Configure **Device platform**, to **windows** and **iOS**.
     - Configure **Sign-in risk**, to **High** and **Medium**.
   - **Grant**:  
     - Require **multi-factor authentication (MFA)**. 
5. Enable Policy by setting it to **On**. Then Create Policy.  


### **Exercise 2: Configure Multi-Factor Authentication (MFA) for Users**  
#### **Estimated Timing: 10 minutes**

#### **Task 1: Enforce MFA through Conditional Access**
1. Navigate to **Azure portal**.  

2. In the search bar at the top of the portal, type **"Entra ID"** and select it from the search results.

2. Under **Manage** select **Security**.

3. Under **Protect**, select **Conditional Access**.

3. Click **+ Create New Policy**, name it **MFA to Admin Portal**.  

4. Configure the following settings:  
   - **Users**: Select "All Users".
   - **Target resources**: Select **Microsoft Admin Portal**
   - *skip networks.*
   - **Conditions**:  
     - Configure **Sign-in risk**, to **High** and **Medium**.
   - **Grant**:  
     - Require **multi-factor authentication (MFA)**. 
5. Enable Policy by setting it to **On**. Then Create Policy. 



### **Exercise 3: Implement Network Segmentation and Private Access**  
#### **Estimated Timing: 20 minutes**

#### **Task 1: Configure Azure Virtual Network (VNet) Segmentation**  
- Open the **Azure Portal** and navigate to **Virtual Networks**.
- Create a new **VNet** to segment workloads logically. Consider your subnets:
  - **Public Subnet**: Only allows external applications and necessary services.
  - **Private Subnet**: For internal workloads like databases and critical services—restrict access strictly.
- Define **Subnet Delegation** if needed, associating subnets with specific Azure services (e.g., Azure App Service or SQL).

#### **Task 2: Apply Network Security Groups (NSGs)**  
- NSGs act as virtual firewalls for your subnet by defining inbound/outbound rules.
- Attach NSGs to each subnet, setting rules such as:
  - **Allow only trusted sources**: Restrict access to internal resources.
  - **Block unauthorized traffic**: Only allow whitelisted IPs and required ports.
  - **Enforce Zero-Trust principles**: Deny unnecessary communication between different subnet types.
- Example NSG rules:
  ```plaintext
  Rule 1: Allow HTTP/HTTPS traffic from trusted corporate IP ranges.
  Rule 2: Deny SSH/RDP access from unknown sources.
  Rule 3: Restrict communication between public and private subnets unless explicitly allowed.
  ```

#### **Task 3: Use Azure Private Link for Secure App Access**  
- Private Link ensures that critical services can only be accessed **privately**, preventing exposure to the internet.
- Steps to implement:
  - Navigate to **Private Link** and create a **Private Endpoint**.
  - Associate it with sensitive workloads such as **Azure SQL Database or Storage Accounts**.
  - Configure **DNS resolution** to ensure private endpoints are used instead of public ones.
  - Ensure applications use the **private endpoint connection** instead of accessing resources directly via the public internet.
- Benefits:
  - **Reduces exposure to external threats**.
  - **Limits unnecessary network access**.
  - **Provides end-to-end encrypted private connections**.


### **Exercise 4: Monitor and Audit Access Logs**  
#### **Estimated Timing: 15 minutes**

#### **Task 1: Enable Azure Monitor and Log Analytics**
1. Navigate to **Azure Monitor** and configure **Diagnostic Settings**.  
2. Enable logging for **sign-in attempts**, **conditional access events**, and **network activity**.  

#### **Task 2: Review Security Logs Using KQL**
1. Open **Log Analytics** and run queries in **Kusto Query Language (KQL)**:
   ```kusto
   SigninLogs
   | where RiskLevel != "None"
   | order by TimeGenerated desc
   ```
2. Investigate suspicious login attempts and adjust policies accordingly.


## **Outcome**
> **Outcome**: By following these steps, you successfully implemented a **Zero-Trust Architecture** in Azure, securing identity access and network traffic. The organization now has a robust **identity-based security model**, protecting against unauthorized access and insider threats.

>**Side Task**: Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.