### GCP Project 01: Role-Based Access Control
## CyberPreacher Edition

## Project Scenario
A small startup recently migrated its infrastructure to GCP. However, they didn’t configure proper security controls. As a result:
- A developer accidentally exposed sensitive data stored in a Cloud Storage bucket to the public.
- An attacker gained access to the cloud environment using stolen credentials because MFA was not enabled.
- The company incurred unexpected costs due to unauthorized resource usage, as there were no billing alerts or monitoring in place.

> For all the resources in this lab, use the **us-east1** region.

## Lab Objectives
In this lab, you will complete the following exercises:
- Exercise 1: Create a Resource Group (aka Project).
- Exercise 2: Create a GCP IAM User and assign the Security Administrator role.
- Exercise 3: Configure Identity and Access Management (IAM) for the Project.
- Exercise 4: Enable GCP Cloud Monitoring and Logging.
- Exercise 5: Set Up Billing Alerts.

## Instructions

### Exercise 1: Create the Project and assign IAM permissions for the Security User on the Project.

#### Estimated timing: 15 minutes

> **Note**: Please ensure that you have created a GCP Free Tier Account at (https://cloud.google.com/free).

In this exercise, you will complete the following tasks:
- Task 1: Create a Project.
- Task 2: Create a GCP IAM User and assign the Security Administrator role.
- Task 3: Configure Identity and Access Management (IAM) for the Project.

#### Task 1: Create a Project

In this task, you will create a project.

1. Start a browser session and sign in to the Google Cloud Console **`https://console.cloud.google.com/`**.

2. In the **Google Cloud Console**, click on the project dropdown at the top to open the project selector.

3. Click **New Project**.

4. In the **New Project** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Project name|**CyberP-Project**|
   |Location|**No organization**|

5. Click **Create**.

6. Verify the new project was created in your GCP console.

#### Task 2: Create a GCP IAM User and assign the Security Administrator role

In this task, you will create a GCP IAM User and assign the Security Administrator role.

1. In the Google Cloud Console at `https://console.cloud.google.com/`.

2. In the **Navigation menu**, go to **IAM & Admin** and select **Service accounts**.

3. On the **Service Accounts** page, select **+ Create Service Account**.

4. On the **Create Service Account** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Service account name|**Bob**|
   |Role|**Security Admin**|
   |Account ID|**Auto-generate**|
   
5. Click **Done**.

6. Copy the **Service Account Key** for Bob.

> **Result**: You created a service account and assigned the Security Admin role.

### Exercise 2: Configure IAM for the Project.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Configure Identity and Access Management (IAM) for the Project.

#### Task 1: Configure Identity and Access Management (IAM) for the Project

In this task, you will assign **Bob** with the **Editor** role for the **CyberP-Project** project.

1. In the **Navigation menu**, go to **IAM & Admin** and select **IAM**.

2. On the **IAM** page, click **+ Add**.

3. In the **Add members** dialog, specify the following:

   |Setting|Value|
   |---|---|
   |New Members|Service account ID of Bob|
   |Roles|Editor|

4. Click **Save**.

> **Result**: You successfully configured IAM for the Project.

### Exercise 3: Enable GCP Cloud Monitoring and Logging and Set Up Billing Alerts.
#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Enable GCP Cloud Monitoring and Logging.
- Task 2: Set Up Billing Alerts.

#### Task 1: Enable GCP Cloud Monitoring and Logging

1. In the **Navigation menu**, go to **Monitoring**.

2. In the **Monitoring** page, select **Create Workspace**.

3. In the **Create Workspace** dialog, specify the following settings:

   |Setting|Value|
   |---|---|
   |Workspace name|**CyberP-Monitor**|
   |Region|**us-east1**|

4. Click **Create Workspace**.

5. In the **Navigation menu**, go to **Logging**.

6. In the **Logging** page, select **Create Log**.

7. In the **Create Log** dialog, specify the following settings:

   |Setting|Value|
   |---|---|
   |Log Name|**CyberP-Logs**|
   |Resource|**Global**|

8. Click **Create Log**.

#### Task 2: Set Up Billing Alerts

1. In the **Navigation menu**, go to **Billing**.

2. In the **Billing** page, select **Budgets & alerts**.

3. On the **Budgets & alerts** page, select **Create Budget**.

4. In the **Create Budget** dialog, specify the following settings:

   |Setting|Value|
   |---|---|
   |Budget Name|**CyberP-Budget**|
   |Amount|Specify a budget amount|
   |Time period|Monthly|

5. Click **Next** and set an alert condition: **100% of Budget**.

6. Add your email address to receive notifications.

7. Review and click **Save**.

> **Result**: You successfully enabled GCP Cloud Monitoring and Logging, and set up billing alerts.

### Summary of Key Security Practices
- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enable logging and monitoring.

> **Side Task**: Create a VM instance and assign **Bob** the **Compute Instance Admin** role for the instance. Then log in as Bob and see what tasks **Bob** can perform with that role. Once done, take a screenshot of the completed task and upload it on LinkedIn with the hashtag #cloudprojectwithcyberpreacher while sharing your experiences around the project.

> **Note**: Ensure to delete all resources created during this project to manage costs.
