---
CyberPreacher Lab:
    title: '01 - Set Up a Secure Cloud Environment'
    difficulty: 'Easy'
---

# Project 01: Role-Based Access Control
# CyberPreacher Edition

## Project scenario

A small startup recently migrated its infrastructure to the Azure cloud. However, they didn’t configure proper security controls. As a result:

- A developer accidentally exposed sensitive data stored in an S3 bucket (AWS) or Cloud Storage (GCP) to the public.
- An attacker gained access to the cloud environment using stolen credentials because MFA was not enabled.
- The company incurred unexpected costs due to unauthorized resource usage, as there were no billing alerts or monitoring in place.

> For all the resources in this lab, we are using the **East US** region.

## Lab objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create a Resource Group. 
- Exercise 2: Create an Azure AD User and assign the Security Administrator role.
- Exercise 3: Configure Role-Based Access Control (RBAC) for Resource Group. 
- Exercise 4: Enable Azure Monitor and Log Analytics.
- Exercise 5: Set Up Cost Alerts.

## Instructions

### Exercise 1: Create the Senior Admins group with the user account Joseph Price as its member. 

#### Estimated timing: 15 minutes
    
    >**Note**: Please ensure that you have created an Azure Free Account at (https://portal.azure.com/), or Subscribe to Azure Student Package (For Students).

In this exercise, you will complete the following tasks:

- Exercise 1: Create a Resource Group. 
- Exercise 2: Create an Azure AD User and assign the Security Administrator role.
- Exercise 3: Configure Role-Based Access Control (RBAC) for Resource Group. 

#### Task 1: Create a Resource Group 

In this task, you will create a resource group. 

1. Start a browser session and sign-in to the Azure portal **`https://portal.azure.com/`**.

    >**Note**: Sign in to the Azure portal using an account that has the Owner or Contributor role in the Azure subscription you are using for this lab and the Global Administrator role in the Microsoft Entra tenant associated with that subscription.

2. In the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Resource groups** and press the **Enter** key.

3. On the **Resource groups** blade of the Azure Portal, select **Create**.

4. On the **Create a resource group** blade, specify the following settings of the **Basics** section:

   |Setting|Value|
   |---|---|
   |Resource group|**CyberP-Project**|
   |Region|**(US) East US**|

5. Click **Review + create**.

6. Click **Create**. 

7. Refresh the **Resource groups** blade to verify the new resource group was created in your Azure portal.

#### Task2: Create an Entra ID User and assign the Security Administrator role

In this task, you will Create an Azure AD User and assign the Security Administrator.

1. In the Azure portal at https://portal.azure.com. 

2. In the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Microsoft Entra ID** and press the **Enter** key.

3. On the **Overview** blade of the Microsoft Entra ID tenant, in the **Manage** section, select **Users**, and then select **+ New user**.

4. On the **New User** blade, ensure that the **Create user** option is selected, and specify the following settings:

   |Setting|Value|
   |---|---|
   |User name|**Bob**|
   |Name|**Bob Saint**|

5. Click on the copy icon next to the **User name** to copy the full user.

6. Ensure that the **Auto-generate** password is selected, select the **Show password** checkbox to identify the automatically generated password. You would need to provide this password, along with the user name to Joseph. 

7. Click **Create**.

8. Refresh the **Users \| All users** blade to verify the new user was created in your Microsoft Entra tenant.

9. In the users list, select **Bob**.

10. In the **Bob** blade, on the left pane select **Assigned roles**.

11. In the **Bob | Assigned roles** blade, select **+ Add assignments**.

12. In the **Add assignments** blade, under **Membership** select role, search and select **Security Administrator**, then select **Next**.

13. Under **Setting**, select **Active** then click on **Assign** below.

> Result: You used the Azure Portal to create a user and assigned the user the Security Administrator role. 

### Exercise 2: Configure RBAC role for the user.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:

- Task 1: Configure Role-Based Access Control (RBAC) for Resource Group.


#### Task 1: Configure Role-Based Access Control (RBAC) for Resource Group.

In this task, you will assign **Bob** with contributor role-based access control(RBAC) role for **CyberP-Project** resource group.

1. In the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Resource groups** and press the **Enter** key.

2. In the **Resource groups** blade, click **CyberP-Project** resource group.

3. In the **CyberP-Project** blade, on the left pane, click **Access control (IAM)**.

4. In the **CyberP-Project | Access control (IAM)** blade. Click **+ Add**, then select **Add role assignment**.

5. In the **Add role assignment** blade, under **Role** tab, click **Privileged administrator roles**.

6. Select the **Contributor** role, and click **Next**.

7. Under the **Member** tab, click **+ Select members**, then search and select **Bob**, then click **Select**. Cick **Next**.

8. Under **Assignment tab** tab, for the **Assignment tab** role, select **Active**.
 >**Note**: Ignore the warning for now: Contributor is a privileged administrator role. We recommend that you create eligible role assignments for this role or use PIM for Groups.

9. Click **Review + Assign**.

> Result: You successfully configured a RBAC role for the Resource Group.

### Exercise 3: Enable Azure Monitor and Log Analytics and Set Up Cost Alerts.
#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:

- Task 1: Enable Azure Monitor and Log Analytics.
- Task 2: Set Up Cost Alerts.

#### Task1: Enable Azure Monitor and Log Analytics.

1. In the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Monitor** and press the **Enter** key.

2. In the **Monitor** blade, on the left pane select **Logs**. Close the **Welcome to Log Analytics** pane.
    >**Note**: If prompted, do the following below, else Skip and go to step 6.
3. Create a Log Analytics Workspace, using the following parameter:

   |Setting|Value|
   |---|---|
   |Resource group|**CyberP-Project**|
   |Name|**MyLog**|
   |Region|**East US**|

4. Select **Review + Create**, then **Create**.

5. Return to **Monitor** blade, on the left pane select **Log**

6. A pane will appear **Select a scope**. Under the **browse** tab, select your *subscription name*, then click **Apply**.

#### Task 2: Set Up Cost Alerts.

1. In the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Cost Management** and press the **Enter** key.

2. In the **Cost Management** blade, on the left pane click **Monitoring**, then select **Budgets**.

3. In the **Cost Management: <your-subscription-name> | Budgets** blade, Click **+ Add**.

4. In the **Create budget** blade, under **Budget Details** use the following parameters:
   |Setting|Value|
   |---|---|
   |Name|**costalert_project**|
   |Amount ($)|**$100**|

5. Click **Next**.

6. In the **Set alerts** tab, under **Alert conditions**: **Type** - Actual, **% of budget** - 100.

7. Under **Alert recipients (email)**, type in your *<primary-mail@microsoft.com>*.

8. Click **Create**.

> Result: You successfully configured Azure Monitor and Log Analytics, and Set Up Cost Alerts.

### Summary of Key Security Practices

- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enabled logging and monitoring.

 >**Side Task**: Create a virtual machine, and assign **Bob** the Virtual machine contributor role for the machine. Then Log in as Bob and see what task **Bob** can perform with that role. Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher while sharing your experiences around the project.