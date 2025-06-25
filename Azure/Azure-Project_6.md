---
CyberPreacher Lab:
    title: '06 - Secure Enterprise App Deployment'
    difficulty: 'Medium'
---


# Project 6: Secure Enterprise App Deployment
## CyberPreacher Edition

---

### **Project Scenario**

A rapidly growing fintech company is transitioning from a legacy on-premises system to cloud in order to modernize its infrastructure and scale operations. During an internal security review, the following issues were discovered:

- User accounts had excessive permissions, increasing the risk of insider threats.
- Secrets and API credentials were stored as plain text in configuration files.
- No multi-factor authentication (MFA) was enforced, leaving critical admin accounts vulnerable.
- There was no centralized logging, making it difficult to trace incidents or access patterns.
- The company exceeded monthly cloud spending due to lack of cost monitoring.

*Your task*: As part of the cloud engineering team, you will be responsible for deploying a secure enterprise application in AWS that resolves these risks by enforcing strict IAM practices, encrypting secrets, integrating monitoring, and setting up budget alerts.


### **Lab Objectives**

- Exercise 1: Create a Resource Group and Enterprise Application.
- Exercise 2: Register the app and configure SSO & Permissions.
- Exercise 3: Enable and enforce Conditional Access & MFA.
- Exercise 4: Secure credentials using Azure Key Vault.

---

### Exercise 1: Resource Setup & Access Controls
**Estimated Timing**: 15 mins

#### Task 1: Create the Resource Group

1. Sign in to Azure Portal.

2. In the top search bar, type **“Resource groups”** and select it.

3. Click **+ Create**.

4. Fill in the details:

   |Setting|Value|
   |---|---|
   |Subscription|**Choose your active subscription**|
   |Resource group name|**CPApp-RG**|
   |Region|**East US**|

5. Select **Review + create**, then click **Create**.

#### Task 2: Create and Assign App Owner Role

1. Navigate to **Microsoft Entra ID > Users > + New User**.

2. Create user:
   - *Username:* `jane.admin@<domain>`  
   - *Name:* `Jane Admin`

3. Assign role:  
   - Go to **Roles and administrators**, search for `Application Administrator`, and assign it to Jane.


### Exercise 2: Register the Enterprise App & Configure Permissions  
**Estimated Timing**: 20 mins

1. Go to [**Entra ID**](https://entra.microsoft.com/)

2. On the left pane, click on **Application > App Registrations**.

3. Select **+ New registration**  

4. Fill in the details:
   |Setting|Value|
   |---|---|
   |Name|**ContosoEnterpriseApp**|
   |Supported account types|**Accounts in this organization directory only**|
   
5. Click on **Register**.

6. Once created, go to **API permissions**.

7. Click on **+ Add a permission**, then select:
   - Microsoft Graph > Delegated permissions > `User.Read`, `Group.Read.All`

7. Click **Grant admin consent** for your org, select **Yes** on the pop-up.

8. Under **Certificates & secrets**, generate a **New client secret** and store it securely.

9. Fill in the details:
   |Setting|Value|
   |---|---|
   |Description|**MySecret**|
   |Expires|**180 days**|

10. Click on **Add**.

11. Copy your newly created secrets and paste it in a notepad, we would need it moving forward.

### Exercise 3: Enable MFA & Conditional Access  
**Estimated Timing**: 15 mins

1. Go to [**Entra ID**](https://entra.microsoft.com/)

2. On the left pane, click on **Protection > Conditional Access**.

3. Select **+ Create New Policy**

4. Fill in the details:
   |Setting|Value|
   |---|---|
   |Name|`Require MFA for App Users`|
   |Assignments|Target user/group (`App Users`)|
   |Cloud apps|`ContosoEnterpriseApp`|
   |Access controls|Grant → Require multi-factor authentication|

5. Enable and save the policy.


### Exercise 4: Secure App Credentials with Azure Key Vault  
**Estimated Timing**: 10 mins

1. On your browser, navigate to the [Azure Portal](https://portal.azure.com/)

2. On the **Search resources, services, and docs**, search for **Key vaults** and select it.

3. Select **+ Create**

4. Fill in the details:
   |Setting|Value|
   |---|---|
   |Subsciption|*Select your subscription*|
   |Resource group|*CPApp-RG*|
   |Key vault name|`Contoso Vault`|
   |Region|*East US*|

5. Select **Review + Create**

6. Once deployed, **Go to resource**.

7. On the left pane, select **Objects > Secrets**

8. Click on **Generate/Import**
   - *Name:* `MySecret`
   - *Value:* Paste client secret from previous step.

3. Update your app configuration to reference this secret via Managed Identity.

> Result: You successfully created and secured an Enterprise Application.

### Summary of Key Security Practices

- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enabled logging and monitoring.

 >**Side Task**: Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.
