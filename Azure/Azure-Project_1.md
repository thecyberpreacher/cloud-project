---
lab:
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

#### Task2: Use PowerShell to create the Junior Admins group and add the user account of Isabel Garcia to the group.

In this task, you will create the Junior Admins group and add the user account of Isabel Garcia to the group by using PowerShell.

1. In the same PowerShell session within the Cloud Shell pane, run the following to **create a new security group** named Junior Admins:
   
   ```powershell
   New-AzureADGroup -DisplayName 'Junior Admins43846135' -MailEnabled $false -SecurityEnabled $true -MailNickName JuniorAdmins
   ```
   
2. In the PowerShell session within the Cloud Shell pane, run the following to **list groups** in your Microsoft Entra tenant (the list should include the Senior Admins and Junior Admins groups)
   
   ```powershell
   Get-AzureADGroup
   ```

3. In the PowerShell session within the Cloud Shell pane, run the following to **obtain a reference** to the user account of Isabel Garcia:

   ```powershell
   $user = Get-AzureADUser -Filter "UserPrincipalName eq 'Isabel-43846135@LODSPRODMCA.onmicrosoft.com'"
   ```

4. In the PowerShell session within the Cloud Shell pane, run the following to add the user account of Isabel to the Junior Admins43846135 group:
   ```powershell
   Add-AzADGroupMember -MemberUserPrincipalName $user.userPrincipalName -TargetGroupDisplayName "Junior Admins43846135"
   ```

5. In the PowerShell session within the Cloud Shell pane, run the following to verify that the Junior Admins43846135 group contains the user account of Isabel:
   
   ```powershell
    Get-AzADGroupMember -GroupDisplayName "Junior Admins43846135"
    ```
   
> Result: You used PowerShell to create a user and a group account, and added the user account to the group account. 

### Exercise 3: Create a Service Desk group containing the user account of Dylan Williams as its member.

#### Estimated timing: 10 minutes

In this exercise, you will complete the following tasks:

- Task 1: Use Azure CLI to create a user account for Dylan Williams.
- Task 2: Use Azure CLI to create the Service Desk group and add the user account of Dylan to the group. 

#### Task 1: Use Azure CLI to create a user account for Dylan Williams.

In this task, you will create a user account for Dylan Williams.

1. In the drop-down menu in the upper-left corner of the Cloud Shell pane, select **Bash**, and, when prompted, click **Confirm**. 

2. In the Bash session within the Cloud Shell pane, run the following to identify the name of your Microsoft Entra tenant:

    ```cli
    DOMAINNAME=$(az ad signed-in-user show --query 'userPrincipalName' | cut -d '@' -f 2 | sed 's/\"//')
    ```

3. In the Bash session within the Cloud Shell pane, run the following to create a user, Dylan Williams. Use *yourdomain*.
 
    ```cli
    az ad user create --display-name "Dylan Williams" --password "Pa55w.rd1234" --user-principal-name Dylan@$DOMAINNAME
    ```
      
4. In the Bash session within the Cloud Shell pane, run the following to list Microsoft Entra ID user accounts (the list should include user accounts of Joseph, Isabel, and Dylan)
	
    ```cli
    az ad user list --output table
    ```

#### Task 2: Use Azure CLI to create the Service Desk group and add the user account of Dylan to the group. 

In this task, you will create the Service Desk group and assign Dylan to the group. 

1. In the same Bash session within the Cloud Shell pane, run the following to create a new security group named Service Desk.

    ```cli
    az ad group create --display-name "Service Desk" --mail-nickname "ServiceDesk"
    ```
 
2. In the Bash session within the Cloud Shell pane, run the following to list the Microsoft Entra ID groups (the list should include Service Desk, Senior Admins, and Junior Admins groups):

    ```cli
    az ad group list -o table
    ```

3. In the Bash session within the Cloud Shell pane, run the following to obtain a reference to the user account of Dylan Williams: 

    ```cli
    USER=$(az ad user list --filter "displayname eq 'Dylan Williams'")
    ```

4. In the Bash session within the Cloud Shell pane, run the following to obtain the objectId property of the user account of Dylan Williams: 

    ```cli
    OBJECTID=$(echo $USER | jq '.[].id' | tr -d '"')
    ```

5. In the Bash session within the Cloud Shell pane, run the following to add the user account of Dylan to the Service Desk group: 

    ```cli
    az ad group member add --group "Service Desk" --member-id $OBJECTID
    ```

6. In the Bash session within the Cloud Shell pane, run the following to list members of the Service Desk group and verify that it includes the user account of Dylan:

    ```cli
    az ad group member list --group "Service Desk"
    ```

7. Close the Cloud Shell pane.

> Result: Using Azure CLI you created a user and a group accounts, and added the user account to the group. 


### Exercise 4: Assign the Virtual Machine Contributor role to the Service Desk group.

#### Estimated timing: 10 minutes

In this exercise, you will complete the following tasks:

- Task 1: Create a resource group. 
- Task 2: Assign the Service Desk Virtual Machine Contributor permissions to the resource group.  

#### Task 1: Create a resource group

1. In the Azure portal, in the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Resource groups** and press the **Enter** key.

2. On the **Resource groups** blade, click **+ Create** and specify the following settings:

   |Setting|Value|
   |---|---|
   |Subscription name|the name of your Azure subscription|
   |Resource group name|**AZ500Lab01**|
   |Location|**East US**|

3. Click **Review + create** and then **Create**.

   >**Note**: Wait for the resource group to deploy. Use the **Notification** icon (top right) to track progress of the deployment status.

4. Back on the **Resource groups** blade, refresh the page and verify your new resource group appears in the list of resource groups.


#### Task 2: Assign the Service Desk Virtual Machine Contributor permissions. 

1. On the **Resource groups** blade, click the **AZ500LAB01** resource group entry.

2. On the **AZ500Lab01** blade, click **Access control (IAM)** in the middle pane.

3. On the **AZ500Lab01 \| Access control (IAM)** blade, click **+ Add** and then, in the drop-down menu, click **Add role assignment**.

4. On the **Add role assignment** blade, specify the following settings and click **Next** after each step:

   |Setting|Value|
   |---|---|
   |Role in the search tab|**Virtual Machine Contributor**|
   |Assign access to (Under Members Pane)|**User, group, or service principal**|
   |Select (+Select Members)|**Service Desk**|

5. Click **Review + assign** twice to create the role assignment.

6. From the **Access control (IAM)** blade, select **Role assignments**.

7. On the **AZ500Lab01 \| Access control (IAM)** blade, on the **Check access** tab, in the **Search by name or email address** text box, type **Dylan Williams**.

8. In the list of search results, select the user account of Dylan Williams and, on the **Dylan Williams assignments - AZ500Lab01** blade, view the newly created assignment.

9. Close the **Dylan Williams assignments - AZ500Lab01** blade.

10. Repeat the same last two steps to check access for **Joseph Price**. 

> Result: You have assigned and checked RBAC permissions. 

**Clean up resources**

> Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs.

1. In the Azure portal, open the Cloud Shell by clicking the first icon in the top right of the Azure Portal. 

2. In the drop-down menu in the upper-left corner of the Cloud Shell pane, select **PowerShell**, and, when prompted, click **Confirm**. 

3. In the PowerShell session within the Cloud Shell pane, run the following to remove the resource group you created in this lab:
  
    ```
    Remove-AzResourceGroup -Name "AZ500LAB01" -Force -AsJob
    ```

4.  Close the **Cloud Shell** pane. 