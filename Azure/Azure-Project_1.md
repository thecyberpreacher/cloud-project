---
CyberPreacher Lab:
    title: '01 - Set Up a Secure Cloud Environment'
    difficulty: 'Easy'
---

# Project 01: Set Up a Secure Cloud Environment
## CyberPreacher Edition

## Project scenario

A small startup recently migrated its infrastructure to the Azure cloud. However, they didn’t configure proper security controls. As a result:

- A developer accidentally exposed sensitive data stored in an Azure Storage to the public.
- An attacker gained access to the cloud environment using stolen credentials because MFA was not enabled.
- The company incurred unexpected costs due to unauthorized resource usage, as there were no billing alerts or monitoring in place.

> For all the resources in this lab, we are using the **East US** region.

## Lab objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create an Entra ID User 
- Exercise 2: Assign the Security Administrator role.
- Exercise 3: Set up Conditional Access for Multi-Factor Authentication (MFA). 
- Exercise 4: Enable Azure Monitor and Log Analytics.
- Exercise 5: Set Up Cost Alerts.

## Instructions

### Exercise 1: Create the Resource Group and assign RBAC permission for the Security User on the group. 

#### Estimated timing: 15 minutes
    
>**Note**: Please ensure that you have created an Azure Account with active subscription credit (https://portal.azure.com/), or Subscribe to Azure Student Package (https://aka.ms/student-account).

In this exercise, you will complete the following tasks:

- Task 1: Create an Entra ID User and assign the Security Administrator role.
- Task 2: Assign Security Administrator role to User
- Task 2: Set up Conditional Access for Multi-Factor Authentication (MFA)

#### Task2: Create an Entra ID User and assign the Security Administrator role

In this task, you will Create an Entra ID User and assign the Security Administrator.

1. Start a browser session and sign-in to the Azure portal **`https://portal.azure.com/`**.

    >**Note**: Sign in to the Azure portal using an account that has the Owner or Contributor role in the Azure subscription you are using for this lab and the Global Administrator role in the Microsoft Entra tenant associated with that subscription.
\

2. In the **Search resources, services, and docs** text box at the top of the Azure portal page, type **Microsoft Entra ID** and press the **Enter** key.

3. On the **Overview** blade of the Microsoft Entra ID tenant, in the **Manage** section, select **Users**, and then select **+ New user**.

4. On the **New User** blade, ensure that the **Create user** option is selected, and specify the following settings:

   |Setting|Value|
   |---|---|
   |User name|**Bob**|
   |Name|**Bob Saint**|
   |Role|**Security Admin**|

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

### Exercise 2: Set Up MultiFactor Authentication (MFA).

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:

- Task 1: Set up Conditional Access for Multi-Factor Authentication (MFA).

#### Task 2: Set up Conditional Access for Multi-Factor Authentication (MFA)

1. In the search bar at the top of the portal, type **"Entra ID"** and select it from the search results.

2. In the Azure Entra ID blade, select **"Security"** from the left-hand menu.

3. Click on **"Conditional Access"** under the **"Security"** section.

4. Click on **"+ New policy"** to create a new Conditional Access policy.

5. **Name**: Provide a name for the policy "Require MFA for All Users".

6. **Assignments**: Select the users or groups this policy applies to. You can choose to apply it to all users or specific groups.

7. **Cloud apps or actions**: Skip.

8. **Conditions**: Skip.

9. **Access controls**: Under **"Grant**, select **"Require multi-factor authentication**.

10. **Session controls**: Optionally, configure session controls such as session timeout and persistent sessions.

11. **Enable policy**: Turn on the policy by toggling the switch to **"On"**.

12. Review the settings to ensure everything is configured correctly.

13. Click **"Create"** to save the policy.

14. Ensure that users are prompted for MFA when they sign in under the conditions specified in the policy.
15. Monitor the sign-in logs to verify that the policy is working as expected.

> By following these steps, you can set up Conditional Access to require MFA for specific sign-in events, enhancing the security of your Azure environment.

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
   |Resource group|**CyberP-Project**|
   |Name|**MyLog**|
   |Budget Amount|**$25**|
   > Leave the rest as default, but I'd advice to analyze what each of them does.
   
5. Click **Next**.

6. In the **Set alerts** tab, under **Alert conditions**: **Type** - Actual, **% of budget** - 100.

7. Under **Alert recipients (email)**, type in your *<primary-mail@microsoft.com>*.

8. Click **Create**.

> Result: You successfully configured Azure Monitor and Log Analytics, and Set Up Cost Alerts.

### Summary of Key Security Practices

- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enabled logging and monitoring.

 >**Side Task**: Create a virtual machine, and assign **Bob** the Virtual machine contributor role for the machine. Then Log in as Bob and see what task **Bob** can perform with that role. Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.