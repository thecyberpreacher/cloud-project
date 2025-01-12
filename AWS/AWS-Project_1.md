### AWS Project 01: Role-Based Access Control
## CyberPreacher Edition

## Project Scenario
A small startup recently migrated its infrastructure to AWS. However, they didn’t configure proper security controls. As a result:
- A developer accidentally exposed sensitive data stored in an S3 bucket to the public.
- An attacker gained access to the cloud environment using stolen credentials because MFA was not enabled.
- The company incurred unexpected costs due to unauthorized resource usage, as there were no billing alerts or monitoring in place.

> For all the resources in this lab, use the **US East (N. Virginia)** region.

## Lab Objectives
In this lab, you will complete the following exercises:
- Exercise 1: Create a Resource Group.
- Exercise 2: Create an AWS IAM User and assign the Security Administrator role.
- Exercise 3: Configure Role-Based Access Control (RBAC) for the Resource Group.
- Exercise 4: Enable AWS CloudWatch and CloudTrail.
- Exercise 5: Set Up Cost Alerts.

## Instructions

### Exercise 1: Create the Resource Group and assign RBAC permission for the Security User on the group

#### Estimated timing: 15 minutes

> **Note**: Please ensure that you have created an AWS Free Tier Account at (https://aws.amazon.com/free), or Subscribe to AWS Educate (For Students).

In this exercise, you will complete the following tasks:
- Task 1: Create a Resource Group.
- Task 2: Create an AWS IAM User and assign the Security Administrator role.
- Task 3: Configure Role-Based Access Control (RBAC) for the Resource Group.

#### Task 1: Create a Resource Group

In this task, you will create a resource group.

1. Start a browser session and sign in to the AWS Management Console **`https://aws.amazon.com/console/`**.

2. In the **Search AWS services** text box at the top of the AWS Management Console page, type **Resource Groups** and press the **Enter** key.

3. On the **Resource Groups** dashboard, select **Create**.

4. On the **Create a resource group** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Resource group name|**CyberP-Project**|
   |Region|**US East (N. Virginia)**|

5. Click **Create Group**.

6. Verify the new resource group was created in your AWS Management Console.

#### Task 2: Create an AWS IAM User and assign the Security Administrator role

In this task, you will create an AWS IAM User and assign the Security Administrator role.

1. In the AWS Management Console at `https://aws.amazon.com/console/`.

2 A. In the **Search AWS services** text box at the top of the AWS Management Console page, type **IAM** and press the **Enter** key.

3. On the **IAM Dashboard**, in the **Access management** section, select **Users**, and then select **Add users**.

4. On the **Add user** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |User name|**Bob**|
   |Password|**Auto-generate password**|
   |AWS Management Console access|Enable|

5. Click **Next: Permissions**.

6. In the **Set permissions** screen, select **Attach policies directly**:

   - Search for and attach the **SecurityAdministrator** policy.

7. Click **Next: Tags**, then **Next: Review**.

8. Review the settings and select **Create user**.

9. Copy the **Access Key ID** and **Secret Access Key** for Bob.

> **Result**: You created a user and assigned the Security Administrator role.

### Exercise 2: Configure RBAC for the Resource Group.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Configure Role-Based Access Control (RBAC) for the Resource Group.

#### Task 1: Configure Role-Based Access Control (RBAC) for the Resource Group

In this task, you will assign **Bob** with the **ResourceGroupAdministrator** role for **CyberP-Project** resource group.

1. In the **Search AWS services** text box at the top of the AWS Management Console page, type **Resource Groups** and press the **Enter** key.

2. In the **Resource Groups** dashboard, click the **CyberP-Project** resource group.

3. In the **CyberP-Project** dashboard, select **Access**.

4. Click **Add permissions**, then select **Policy generator**.

5. Create a custom policy granting **ResourceGroupAdministrator** access for this resource group.

6. Attach the new policy to **Bob**.

> **Result**: You successfully configured RBAC for the Resource Group.

### Exercise 3: Enable AWS CloudWatch and CloudTrail and Set Up Cost Alerts.
#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Enable AWS CloudWatch and CloudTrail.
- Task 2: Set Up Cost Alerts.

#### Task 1: Enable AWS CloudWatch and CloudTrail

1. In the **Search AWS services** text box, type **CloudWatch** and press the **Enter** key.

2. In the **CloudWatch** dashboard, select **Logs**, then set up a log group.

3. Create a **Log Group** with the following parameters:

   |Setting|Value|
   |---|---|
   |Log Group Name|**CyberP-Logs**|
   |Region|**US East (N. Virginia)**|

4. In the **CloudWatch** dashboard, select **Alarms**, then configure necessary CloudWatch Alarms.

5. Next, enter **CloudTrail** in the **Search AWS services** text box and press Enter.

6. In the **CloudTrail** dashboard, select **Trails** and create a new trail using default settings.

#### Task 2: Set Up Cost Alerts

1. In the **Search AWS services** text box, type **Cost Management** and press the **Enter** key.

2. In the **Cost Management** dashboard, select **Budgets**, then create a new budget.

3. On the **Set Budget** page, specify the following settings:

   |Setting|Value|
   |---|---|
   |Budget name|**CyberP-Budget**|
   |Cost/Usage type|**Cost Budget**|
   |Period|**Monthly**|

4. Click **Next** and set an alert condition: **100% of Budget**.

5. Add your email address to receive notifications.

6. Review and click **Create Budget**.

> **Result**: You successfully enabled AWS CloudWatch and CloudTrail, and set up cost alerts.

### Summary of Key Security Practices

- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enable logging and monitoring.

> **Side Task**: Create an EC2 instance and assign **Bob** the **EC2InstanceAdministrator** role for the instance. Then log in as Bob and see what tasks **Bob** can perform with that role. Once done, take a screenshot of the completed task and upload it on LinkedIn with the hashtag #cloudprojectwithcyberpreacher while sharing your experiences around the project.

> **Note**: Ensure to delete all resources created during this project to manage costs.