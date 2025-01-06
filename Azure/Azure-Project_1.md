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

### Exercise 2: Configure RBAC role for the user and Enable Azure Monitor and Log Analytics.

#### Estimated timing: 10 minutes

In this exercise, you will complete the following tasks:

- Task 1: Configure Role-Based Access Control (RBAC) for Resource Group.
- Task 2: Enable Azure Monitor and Log Analytics.
- Task 3: Set Up Cost Alerts. 

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

### Exercise 3: Create a Service Desk group containing the user account of Dylan Williams as its member.

#### Task2: Enable Azure Monitor and Log Analytics.

1. 

#### Estimated timing: 10 minutes




### Exercise 4: Assign the Virtual Machine Contributor role to the Service Desk group.

#### Estimated timing: 10 minutes


#### Task 1: Create a resource group
#### Task 2: Assign the Service Desk Virtual Machine Contributor permissions. 