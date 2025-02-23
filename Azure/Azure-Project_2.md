---
CyberPreacher Lab:
    title: '02 - Encrypt Data at Rest and in Transit'
    difficulty: 'Easy'
---

# Project 02: Encrypt Data at Rest and in Transit
## CyberPreacher Edition

## Project scenario

You are a cybersecurity consultant for a FinTech company that handles sensitive financial data, including customer transactions, account details, and personal information. The company wants to ensure that all sensitive data is encrypted both at rest and in transit to comply with industry regulations and protect against data breaches.

Project Objective: Implement encryption for data at rest and in transit using Azure Cloud services to ensure the security and integrity of the company's financial data.

> For all the resources in this lab, we are using the **East US** region.

## Lab objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create an Azure Storage Account 
- Exercise 2: Enable Azure Storage Service Encryption.
- Exercise 3: Upload a File to Azure Storage. 
- Exercise 4: Enable HTTPS for Data in Transit.
- Exercise 5: Monitor and Audit.

## Instructions

### Exercise 1: Create an Azure Storage Account. 

#### Estimated timing: 5 minutes
    
>**Note**: Please ensure that you have created an Azure Account with active subscription credit (https://portal.azure.com/), or Subscribe to Azure Student Package (https://aka.ms/student-account).

In this exercise, you will complete the following tasks **Create an Azure Storage**.

#### Task 1: Create an Azure Account

In this task, you will Create an Azure Storage account.

1. Navigate to the Azure portal at (https://portal.azure.com).

2. In the search bar at the top of the portal, type **"Storage account"** and select it from the search results.

3. Click **Create**.

4. In the **Create a storage account** blade, under the **Basics** tab, fill in the following items:

   |Setting|Value|
   |---|---|
   |Subscription|*<your subscription>*|
   |Resource group|**CyberP-Project**|
   |Storage account name|**demoproject01**|
   |Region|**(US) East US**|
   |Primary service|**Azure Files**|
   |Performance|**Standard**|
   |Redundancy|**Locally-redundant storage(LRS)**|

5. Click **Next**, notice the **Advanced** tab, leave as default.

6. Click **Next**, notice the **Networking** tab, leave as default.

7. Click **Next**, notice the **Data protection** tab, leave as default.

8. Click **Next**, notice the **Encryption** tab, leave as default.

9. Click **Next**, notice the **Tag** tab, leave as default.

10. Click **Review + create**. Click on **Create**.


> Result: You used the Azure Portal to create an Azure Storage Account.

### Exercise 2: Enable Azure Storage Service Encryption.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:

- Task 1: Enable Azure Storage Service Encryption.

#### Task 1: Enable Azure Storage Service Encrytion.

> Note: As part of Azure security, Storage service encryption is enabled by default (Secure by design). Go through the steps to understand better.

1. In the search bar at the top of the portal, type **"Storage Accounts"** and select it from the search results.

2. In the **Storage accounts**, select **"demoproject01"** from the list.

3. On the left blade, select **Security + Networking** then select **Encryption**.

4. Ensure that **Microsoft-managed keys** is selected, and if you prefer to use your own encryption, select **Customer-managed keys** and configure Azure Key vault.

5. Click **Save**.

> You have sucessfully enabled Azure Storage Service Encryption.

### Exercise 3: Upload a File to Azure Storage.
#### Estimated timing: 10 minutes

In this exercise, you will complete the following tasks:

- Task 1: Create a Container.
- Task 2: Upload a File.

#### Task 1: Create a Container.

1. Navigate to your <storage account> **"demoproject01"**.

2. On the left pane, click on **Data storage** then click on **Container**.

3. Click **+ Container**.

4. Place in a name for the new container, type **financial-data**.

5. Observe the **Advanced** tab, then click on **Create**.

6. *You're brought back to the **Container** blade*, refresh the containers.

7. Notice your newly created conatainer.

#### Task 2: Upload a File.

1. Click on the container you just created.

2. In the **financial-data** blade, Click **Upload**.

3. Click on **Browse for files**, then select any *small sized* file in your system.pload

4. Click **Upload**, *Notice the **Access Control (IAM)** tab, where you can set specific permission for other user*.

5. On the **Overview** tab, notice your file got uploaded into Azure.

6. The current **Authentication method** is Access key, notice you can also switch to **Microsoft Entra user account**.

> Note: The file will be automatically encrypted at rest by Azure Storage Service Encryption.

### Exercise 4: Enable HTTPS for Data in Transit.
#### Estimated timing: 15 minutes

In this exercise, you will complete the following tasks:

- Task 1: Require Secure Transfer.
- Task 2: Use Azure Storage Explorer to Download the File.

#### Task 1: Require Secure Transfer.

1. Navigate back to your <storage account> **"demoproject01"**.

2. On the left pane, click on **Settings** then click on **Configuration**.

3. Observe the different **Configurations**, ensure that the **Secure transfer required** is set to **Enabled**.

4. Notice the **Alow Blob anonymous access** configuration. (For more security, leave as Disabled, but try out the other options).
> Note: This ensures that all data transfers to and from your storage account use HTTPS, encrypting data in transit.

#### Task 2: Use Azure Storage Explorer to Download the File.

1. On a new tab, Go to the [Azure Storage Explorer download page](https://azure.microsoft.com/en-us/products/storage/storage-explorer/)

2. Download and install the application for your operating system (Windows, macOS, or Linux).

3. Open **Azure Storage Explorer** and accept License Agreement.

4. Click **Sign in with Azure**, then select **Azure**. Click **Next**.

5. *You will be redirected to your browser to sign in*, follow the prompt and sign in.

6. Back to **Azure Storage Explorer**, Select the subscription click **Open Explorer**.

7. Click on the drop-down close to your subcription, and see storage account.

8. On the right pane, click **Connect to Azure resources**.

9. Select **Storage account or service**, and select **Connection string (Key or SAS)**. Click **Next**.
> You are required to provide an **account name** and a **connection string**.

10. Go to the Azure portal, click on **Storage accounts** from the dashboard and select **demoproject01**.

11. In the **demoproject01** blade. On the left pane, select **Security + networking** and click **Access keys**.

12. Copy both the **Storage account name** and **Connection string** for **key1**, and paste in your **Storage Explorer**.

13. Click **Connect**.
> Your Storage Explorer is refreshed and you can now see your storage containers.

13. Click on **Blob Containers** drop-down, and select **financial-data**.

14. Select the file you uploaded, and click **Download**, and choose a location on your local machine to save the file.

> Result: The file is now securely downloaded using HTTPS.

### Exercise 5: Monitor and Audit.
#### Estimated timing: 10 minutes

In this exercise, you will complete the following tasks:

- Task 1: Enable Logging and Monitoring.
- Task 2: Review Access Log.

#### Task 1: Enable Logging and Monitoring.

1. Navigate back to your <storage account> **"demoproject01"**.

2. On the left pane, click on **Monitoring** then click on **Diagnostic settings**.

3. Click on **demoproject01** from the list.

4. Click **+ Add diagnostic setting**.

5. For the **Diagnostic setting name**, type **proj-diag**. Under **Metrics** select *Transaction*, under **Destination details** select *Send to Log Analytics workspace*.

6. Click **Save**.
> Result: Diagnostics settings has been enabled to capture logs and metrics and can be analyzed in the Log Analytics workspace.

#### Task 2: Review Access Logs

1. Regularly review access logs to ensure there are no unauthorized access attempts.

2. Use Azure Security Center to get recommendations and insights.

### Outcome:

>**Outcome**: By implementing these steps, the FinTech company ensures that their sensitive financial data is securely stored and transmitted, complying with industry regulations and protecting against potential data breaches. This project scenario highlights the importance of data encryption in maintaining the security and integrity of critical information.

You habe successfully completed this project, ensure you share your experiences and screenshots on LinkedIn using hashtag #cloudprojectwithcyberpreacher #CPwCP.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.