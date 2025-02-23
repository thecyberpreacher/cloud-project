Sure! Here's the GCP counterpart for your Azure project:

---
CyberPreacher Lab:
    title: '02 - Encrypt Data at Rest and in Transit'
    difficulty: 'Easy'
---

# Project 02: Encrypt Data at Rest and in Transit
## CyberPreacher Edition

## Project Scenario

You are a cybersecurity consultant for a FinTech company that handles sensitive financial data, including customer transactions, account details, and personal information. The company wants to ensure that all sensitive data is encrypted both at rest and in transit to comply with industry regulations and protect against data breaches.

**Project Objective:** Implement encryption for data at rest and in transit using Google Cloud Platform (GCP) services to ensure the security and integrity of the company's financial data.

> For all the resources in this lab, we are using the **us-central1 (Iowa)** region.

## Lab Objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create a Google Cloud Storage Bucket
- Exercise 2: Enable Server-Side Encryption (SSE)
- Exercise 3: Upload a File to Google Cloud Storage
- Exercise 4: Enable HTTPS for Data in Transit
- Exercise 5: Monitor and Audit

## Instructions

### Exercise 1: Create a Google Cloud Storage Bucket

#### Estimated Timing: 5 minutes

>**Note**: Please ensure that you have created a GCP Account with active subscription (https://cloud.google.com/), or sign up for the GCP Free Tier.

In this exercise, you will create a Google Cloud Storage bucket.

#### Task 1: Create a Google Cloud Storage Bucket

1. Navigate to the Google Cloud Console at [GCP Console](https://console.cloud.google.com/).

2. In the **Navigation menu**, select **Cloud Storage** and then click on **Buckets**.

3. Click **Create bucket**.

4. In the **Create a bucket** page, fill in the following details:

   |Setting|Value|
   |---|---|
   |Bucket name|**cyberpreacher-demo-bucket**|
   |Region|**us-central1 (Iowa)**|
   |Default storage class|**Standard**|
   |Access control|**Fine-grained**|

5. Click **Create**.

> Result: You have successfully created a Google Cloud Storage bucket.

### Exercise 2: Enable Server-Side Encryption (SSE)

#### Estimated Timing: 5 minutes

In this exercise, you will enable Server-Side Encryption for your Google Cloud Storage bucket.

#### Task 1: Enable Server-Side Encryption

1. Navigate to your Cloud Storage bucket **cyberpreacher-demo-bucket**.

2. Click on the **Configuration** tab.

3. In the **Encryption** section, select **Google-managed key**.

4. Click **Save**.

> Result: You have successfully enabled Server-Side Encryption for your Google Cloud Storage bucket.

### Exercise 3: Upload a File to Google Cloud Storage

#### Estimated Timing: 10 minutes

In this exercise, you will upload a file to your Google Cloud Storage bucket.

#### Task 1: Upload a File

1. Navigate to your Cloud Storage bucket **cyberpreacher-demo-bucket**.

2. Click on the **Upload files** button.

3. Select a small file from your system (e.g., "financial-report.pdf").

4. Click **Open** to start the upload.

> Result: The file is now uploaded and encrypted at rest using Server-Side Encryption.

### Exercise 4: Enable HTTPS for Data in Transit

#### Estimated Timing: 10 minutes

In this exercise, you will ensure that data is transferred securely using HTTPS.

#### Task 1: Ensure HTTPS for Data Transfer

1. When accessing the file, ensure you use the HTTPS URL provided by Google Cloud Storage.

2. Example: https://storage.googleapis.com/cyberpreacher-demo-bucket/financial-report.pdf

#### Task 2: Use gsutil to Download the File Securely

1. Download and install the Google Cloud SDK from the [GCP SDK download page](https://cloud.google.com/sdk/docs/install).

2. Open a terminal or command prompt and configure the SDK with your credentials using the command:
   ```bash
   gcloud init
   ```

3. Download the file using the gsutil command:
   ```bash
   gsutil cp gs://cyberpreacher-demo-bucket/financial-report.pdf .
   ```

> Result: The file is now securely downloaded using HTTPS.

### Exercise 5: Monitor and Audit

#### Estimated Timing: 10 minutes

In this exercise, you will enable logging and monitoring for your Google Cloud Storage bucket.

#### Task 1: Enable Stackdriver Logging

1. Navigate to the **Logging** section of the Google Cloud Console.

2. Click on **Log Router**.

3. Click **Create Sink**.

4. In the **Create a Sink** page, fill in the following details:

   |Setting|Value|
   |---|---|
   |Sink name|**my-log-sink**|
   |Choose logs to include in sink|**Include only the logs you specify**|
   |Select a destination|**Cloud Storage Bucket**|
   |Bucket name|**cyberpreacher-logs**|

5. Click **Create**.

> Result: Stackdriver Logging is now enabled for your Google Cloud Storage bucket.

#### Task 2: Review Access Logs

1. Regularly review access logs stored in the log bucket.

2. Use Stackdriver Monitoring to get detailed logging information about API calls made to your Google Cloud Storage bucket.

### Outcome

By implementing these steps, the FinTech company ensures that their sensitive financial data is securely stored and transmitted, complying with industry regulations and protecting against potential data breaches. This project scenario highlights the importance of data encryption in maintaining the security and integrity of critical information.

You have successfully completed this project. Ensure you share your experiences and screenshots on LinkedIn using the hashtags #cloudprojectwithcyberpreacher #CPwCP.

>**Note**: Ensure to delete all resources created during this project to manage costs.