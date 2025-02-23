Here's the AWS counterpart for your Azure project:

---
CyberPreacher Lab:
    title: '02 - Encrypt Data at Rest and in Transit'
    difficulty: 'Easy'
---

# Project 02: Encrypt Data at Rest and in Transit
## CyberPreacher Edition

## Project Scenario

You are a cybersecurity consultant for a FinTech company that handles sensitive financial data, including customer transactions, account details, and personal information. The company wants to ensure that all sensitive data is encrypted both at rest and in transit to comply with industry regulations and protect against data breaches.

**Project Objective:** Implement encryption for data at rest and in transit using AWS Cloud services to ensure the security and integrity of the company's financial data.

> For all the resources in this lab, we are using the **US East (N. Virginia)** region.

## Lab Objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create an S3 Bucket
- Exercise 2: Enable Server-Side Encryption (SSE)
- Exercise 3: Upload a File to S3
- Exercise 4: Enable HTTPS for Data in Transit
- Exercise 5: Monitor and Audit

## Instructions

### Exercise 1: Create an S3 Bucket

#### Estimated Timing: 5 minutes
    
>**Note**: Please ensure that you have created an AWS Account with active subscription (https://aws.amazon.com/), or sign up for the AWS Free Tier.

In this exercise, you will create an S3 bucket.

#### Task 1: Create an S3 Bucket

1. Navigate to the AWS Management Console at [AWS Management Console](https://aws.amazon.com/console/).

2. In the **Search** bar at the top of the console, type **S3** and select **S3** from the search results.

3. Click **Create bucket**.

4. In the **Create bucket** page, fill in the following details:

   |Setting|Value|
   |---|---|
   |Bucket name|**cyberpreacher-demo-bucket**|
   |Region|**US East (N. Virginia)**|
   |Block all public access|**Checked**|

5. Click **Create bucket**.

> Result: You have successfully created an S3 bucket.

### Exercise 2: Enable Server-Side Encryption (SSE)

#### Estimated Timing: 5 minutes

In this exercise, you will enable Server-Side Encryption for your S3 bucket.

#### Task 1: Enable Server-Side Encryption

1. Navigate to your S3 bucket **cyberpreacher-demo-bucket**.

2. Click on the **Properties** tab.

3. Scroll down to the **Default encryption** section.

4. Select **Enable** and choose **Amazon S3 key (SSE-S3)**.

5. Click **Save changes**.

> Result: You have successfully enabled Server-Side Encryption for your S3 bucket.

### Exercise 3: Upload a File to S3

#### Estimated Timing: 10 minutes

In this exercise, you will upload a file to your S3 bucket.

#### Task 1: Upload a File

1. Navigate to your S3 bucket **cyberpreacher-demo-bucket**.

2. Click on the **Upload** button.

3. Click **Add files** and select a small file from your system (e.g., "financial-report.pdf").

4. Click **Upload**.

> Result: The file is now uploaded and encrypted at rest using Server-Side Encryption (SSE).

### Exercise 4: Enable HTTPS for Data in Transit

#### Estimated Timing: 10 minutes

In this exercise, you will ensure that data is transferred securely using HTTPS.

#### Task 1: Ensure HTTPS for Data Transfer

1. When accessing the file, ensure you use the HTTPS URL provided by S3.

2. Example: https://cyberpreacher-demo-bucket.s3.amazonaws.com/financial-report.pdf

#### Task 2: Use AWS CLI to Download the File Securely

1. Download and install the AWS CLI from the [AWS CLI download page](https://aws.amazon.com/cli/).

2. Configure the AWS CLI with your credentials using the command:
   ```bash
   aws configure
   ```

3. Download the file using the AWS CLI:
   ```bash
   aws s3 cp s3://cyberpreacher-demo-bucket/financial-report.pdf .
   ```

> Result: The file is now securely downloaded using HTTPS.

### Exercise 5: Monitor and Audit

#### Estimated Timing: 10 minutes

In this exercise, you will enable logging and monitoring for your S3 bucket.

#### Task 1: Enable S3 Bucket Logging

1. Navigate to your S3 bucket **cyberpreacher-demo-bucket**.

2. Click on the **Properties** tab.

3. Scroll down to the **Server access logging** section.

4. Click **Edit** and select **Enable**.

5. Choose a target bucket for storing the logs and specify a target prefix.

6. Click **Save changes**.

> Result: Server access logging is now enabled for your S3 bucket.

#### Task 2: Review Access Logs

1. Regularly review access logs stored in the target bucket.

2. Use AWS CloudTrail to get detailed logging information about API calls made to your S3 bucket.

### Outcome

By implementing these steps, the FinTech company ensures that their sensitive financial data is securely stored and transmitted, complying with industry regulations and protecting against potential data breaches. This project scenario highlights the importance of data encryption in maintaining the security and integrity of critical information.

You have successfully completed this project. Ensure you share your experiences and screenshots on LinkedIn using the hashtags #cloudprojectwithcyberpreacher #CPwCP.

>**Note**: Ensure to delete all resources created during this project to manage costs.