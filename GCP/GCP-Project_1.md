# Project 01: Set Up a Secure Cloud Environment
## CyberPreacher Edition

## Project scenario

A small startup recently migrated its infrastructure to the GCP cloud. However, they didn’t configure proper security controls. As a result:

- A developer accidentally exposed sensitive data stored in a GCS bucket to the public.
- An attacker gained access to the cloud environment using stolen credentials because MFA was not enabled.
- The company incurred unexpected costs due to unauthorized resource usage, as there were no billing alerts or monitoring in place.

> For all the resources in this lab, we are using the **US East (N. Virginia)** region.

## Lab objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create an IAM User
- Exercise 2: Assign the Security Administrator role.
- Exercise 3: Set up Conditional Access for Multi-Factor Authentication (MFA).
- Exercise 4: Enable Google Cloud Monitoring and Logging.
- Exercise 5: Set Up Cost Alerts.

## Instructions

### Exercise 1: Create the Resource Group (Project) and assign IAM permissions for the Security User on the Project.

#### Estimated timing: 15 minutes

> **Note**: Please ensure that you have created a GCP Account with active subscription credit (https://cloud.google.com/).

In this exercise, you will complete the following tasks:

- Task 1: Create an IAM User and assign the Security Administrator role.
- Task 2: Assign the Security Administrator role to User.
- Task 3: Set up Conditional Access for Multi-Factor Authentication (MFA).

#### Task 1: Create an IAM User and assign the Security Administrator role

1. Start a browser session and sign in to the GCP Console **`https://console.cloud.google.com/`**.

2. In the **Search for products and resources** text box at the top of the GCP Console page, type **IAM** and press the **Enter** key.

3. On the **IAM** page, click **Add** to create a new user.

4. On the **Create user** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |User name|**Bob**|
   |Email|**bob@example.com**|
   |Password|**Auto-generate password**|
   |Access type|**Create custom access**|

5. Click **Create**.

6. Copy the **Access key ID** and **Secret access key** for Bob.

7. On the **IAM** page, search for **Bob** and click on his name.

8. Click **Edit** and under **Roles**, click **Add role**.

9. Search for **Security Administrator** and click **Add**.

10. Click **Save**.

> **Result**: You created a user and assigned the Security Administrator role.

### Exercise 2: Configure IAM for the Project.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:

- Task 1: Configure Identity and Access Management (IAM) for the Project.

#### Task 1: Configure Identity and Access Management (IAM) for the Project

In this task, you will assign **Bob** with the **Project Owner** role for the **CyberP-Project** project.

1. In the **Search for products and resources** text box, type **IAM** and press the **Enter** key.

2. On the **IAM** page, click **Add** to create a new role binding.

3. On the **Add role binding** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Project|**CyberP-Project**|
   |Role|**Project Owner**|
   |Member|**bob@example.com**|

4. Click **Save**.

> **Result**: You successfully configured IAM for the Project.

### Exercise 3: Enable Google Cloud Monitoring and Logging and Set Up Cost Alerts.
#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:

- Task 1: Enable Google Cloud Monitoring and Logging.
- Task 2: Set Up Cost Alerts.

#### Task 1: Enable Google Cloud Monitoring and Logging

1. In the **Search for products and resources** text box, type **Monitoring** and press the **Enter** key.

2. On the **Monitoring** page, click **Create a new project**.

3. On the **Create a new project** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Project ID|**CyberP-Monitoring**|
   |Project name|**CyberP Monitoring**|
   |Organization ID|**Your organization ID**|

4. Click **Create**.

5. On the **Monitoring** page, click **Create a new workspace**.

6. On the **Create a new workspace** page, specify the following settings:

   |Setting|Value|
   |---|---| 
   |Workspace name|**CyberP-Logs**|
   |Location|**US East (N. Virginia)**|

7. Click **Create**.

> **Result**: You enabled Google Cloud Monitoring and Logging.

#### Task 2: Set Up Cost Alerts

1. In the **Search for products and services** text box, type **Billing** and press the **Enter** key.

2. On the **Billing** page, click **Create a new budget**.

3. On the **Create a new budget** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Budget name|**CyberP-Budget**|
   |Budget amount|**$100**|
   |Time period|**Monthly**|

4. Click **Next**.

5. Under **Alerts**, click **Add alert**.

6. On the **Add alert** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Alert name|**Cost Alert**|
   |Alert threshold|**100%**|
   |Email address|**bob@example.com**|

7. Click **Create**.

> **Result**: You successfully set up cost alerts.

### Summary of Key Security Practices

- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enable logging and monitoring.

> **Side Task**: Create a Compute Engine instance and assign **Bob** the **Compute Engine Admin** role for the instance. Then log in as Bob and see what tasks **Bob** can perform with that role. Once done, take a screenshot of the completed task and upload it on LinkedIn with the hashtag #cloudprojectwithcyberpreacher while sharing your experiences around the project.

> **Note**: Ensure to delete all resources created during this project to manage costs.
