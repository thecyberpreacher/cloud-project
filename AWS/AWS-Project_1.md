### AWS Project 01: Set Up a Secure Cloud Environment
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

> **Note**: Please ensure that you have created an AWS Account with an active credit at (https://aws.amazon.com/), or Subscribe to AWS Educate (For Students).

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

#### Task 2: Create a GCP IAM User and assign the Security Administrator role

In this task, you will create a GCP IAM User and assign the Security Administrator role.

1. Navigate to IAM: Log into AWS Management Console and search for IAM.

2. Create a User:

3. Go to **Users** and click **Add Users**.

4. Provide a username (e.g., Bob).

5. Select Access Key - **Programmatic Access and AWS Management Console Access**.

6. Assign Permissions: and Choose **Attach policies directly**.

8. Search for and select the policy **SecurityAdministrator**.

9. Complete User Creation: Review details, create the user, and download the credentials file.

> **Result**: You created a service account and assigned the Security Admin role.

### Exercise 2: Enable MFA for IAM User.

#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Enable MFA for IAM User.
- Task 2: Configure IAM Policies.

#### Task 1: Enable MFA for IAM User.

In this task, you will enable Multifactor Authentication for the user **Bob**.

1. In the **IAM dashboard**, select the **Users** tab.

2. Click on **Bob**.

3. Under the **Security credentials** tab, locate the Assigned MFA device section and click **Manage**.

4. Choose **Virtual MFA device** and click **Continue**.

5. Use an MFA app like **Google Authenticator** to scan the QR code or manually enter the secret key.

6. Enter two consecutive authentication codes from the app to complete the setup.

7. Click **Assign MFA** to save changes.

> **Result**: You successfully enabled MFA for **Bob**.

#### Task 2: Configure IAM Policies.

1. Navigate to the **IAM service**.

2. Click **Policies** from the left-hand menu and choose **Create policy**.

3. Use the Visual editor or the JSON tab to define permissions. Example JSON for S3 read-only access:

``{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:Get*",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}``

4. Click Next: **Tags**, **add optional metadata**, and proceed to **Next: Review**.

5. Name the policy (e.g., S3ReadOnlyPolicy), add an optional description, and click Create policy.

6. Assign the policy to users, groups, or roles as required.

### Exercise 3: Enable AWS CloudWatch and CloudTrail and Set Up Cost Alerts.
#### Estimated timing: 5 minutes

In this exercise, you will complete the following tasks:
- Task 1: Enable AWS CloudTrail.
- Task 2: Set Up Cost Alerts.

#### Task 1: Enable AWS CloudTrail

1. Open the **CloudTrail service** in the AWS Management Console.

2. Click **Create trail** and provide a name (e.g., ***DefaultTrail***).

3. For **Storage location**, create a new S3 bucket, and fill in the name of the bucket.

4. Enable **Log file validation** to ensure log integrity.

5. Enable **CloudWatch Logs** for real-time log monitoring (optional).

6. Review settings and click **Create trail**.

#### Task 2: Set Up Cost Alerts

1. Go to the **Billing Dashboard** from the AWS Management Console.

2. In the left-hand menu, select **Billing preferences**.

3. Enable **Receive Billing Alerts** and click **Save preferences**.

4. Navigate to the **CloudWatch service** to create billing alarms:

5. Select **Alarms > Create alarm**.

6. Click Select **metric > Billing > Total Estimated Charge**.

7. Set **conditions** (e.g., Threshold type = Static, Threshold value = $50).

8. Configure **actions** (e.g., send an email notification via Amazon SNS).

9. Review and click Create alarm.

> **Result**: You successfully enabled AWS CloudWatch, and set up cost alerts.

### Summary of Key Security Practices

- **IAM**: Use least privilege principles for IAM roles and policies.
- **Logging**: Enable logging and monitoring.

> **Side Task**: Create an EC2 instance and assign **Bob** the **EC2InstanceAdministrator** role for the instance. Then log in as Bob and see what tasks **Bob** can perform with that role. Once done, take a screenshot of the completed task and upload it on LinkedIn with the hashtag #cloudprojectwithcyberpreacher while sharing your experiences around the project.

> **Note**: Ensure to delete all resources created during this project to manage costs.