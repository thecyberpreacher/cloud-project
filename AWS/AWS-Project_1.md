# Project 01: Set Up a Secure Cloud Environment
## CyberPreacher Edition

## Project scenario

A small startup recently migrated its infrastructure to the AWS cloud. However, they didn’t configure proper security controls. As a result:

- A developer accidentally exposed sensitive data stored in an S3 bucket to the public.
- An attacker gained access to the cloud environment using stolen credentials because MFA was not enabled.
- The company incurred unexpected costs due to unauthorized resource usage, as there were no billing alerts or monitoring in place.

> For all the resources in this lab, we are using the **US East (N. Virginia)** region.

## Lab objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create an IAM User
- Exercise 2: Assign the Security Administrator role.
- Exercise 3: Set up Conditional Access for Multi-Factor Authentication (MFA).
- Exercise 4: Enable AWS CloudWatch and CloudTrail.
- Exercise 5: Set Up Cost Alerts.

## Instructions

### Exercise 1: Create the Resource Group (Project) and assign IAM permissions for the Security User on the Project.

#### Estimated timing: 15 minutes

> **Note**: Please ensure that you have created an AWS Account with an active credit at (https://aws.amazon.com/), or apply for AWS free credit ([Here](https://pages.awscloud.com/GLOBAL_NCA_LN_ARRC-program-A300-2023.html)).

In this exercise, you will complete the following tasks:
- Task 1: Create an IAM User and assign the Security Administrator role.
- Task 2: Assign the Security Administrator role to User.
- Task 3: Set up Conditional Access for Multi-Factor Authentication (MFA).

#### Task 1: Create an IAM User and assign the Security Administrator role

1. Start a browser session and sign in to the AWS Management Console **`https://aws.amazon.com/console/`**.

2. In the **Search AWS services** text box at the top of the AWS Management Console page, type **IAM** and press the **Enter** key.

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

### Exercise 2: Configure IAM for the Project.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Configure Identity and Access Management (IAM) for the Project.

#### Task 1: Configure Identity and Access Management (IAM) for the Project

In this task, you will assign **Bob** with the **ResourceGroupAdministrator** role for the **CyberP-Project** project.

 Step 1: Create a Custom IAM Policy

1. *Go to*: AWS Console → *IAM* → *Policies* → *Create policy*.
2. *Select the JSON tab* and paste the following:

```{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "resource-groups:*",
                "tag:GetResources",
                "tag:GetTagKeys",
                "tag:GetTagValues"
            ],
            "Resource": "*"
        }
    ]
}

> 🔐 You can restrict `"Resource": "*"` to your specific group ARN if needed (e.g., `arn:aws:resource-groups:us-east-1:123456789012:group/CyberP-Project`).

3. Click *Next*, name it something like: `CyberPProjectAdminPolicy`.
4. Click *Create policy*.

Step 2: Attach the Policy to Bob

1. Go to *IAM* → *Users* → Click on *Bob*.
2. Go to the *Permissions* tab.
3. Click *Add permissions* → *Attach policies directly*.
4. Find and select *CyberPProjectAdminPolicy*.
5. Click *Next*, then *Add permissions*.

> **Result**: You successfully configured IAM for the Project.

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