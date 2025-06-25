---
CyberPreacher Lab:
    title: '06 - Secure Enterprise App Deployment'
    difficulty: 'Medium'
---


# Project 6: Secure Enterprise App Deployment
## CyberPreacher Edition

---

### Project Scenario

A fast-growing fintech company has begun migrating its monolithic, on-premises application infrastructure to Google Cloud Platform. During a pre-launch security audit, several concerns were uncovered:

- IAM accounts had excessive roles assigned, increasing potential for lateral movement and insider threats.
- API keys and credentials were hardcoded in app code and stored in repositories.
- Administrative users did not have two-factor authentication enforced.
- No centralized audit logging was configured, preventing traceability of critical operations.
- Cloud spending exceeded forecasts due to lack of budget alerts and monitoring.

**Your task**: As a cloud engineer, you will deploy a secure internal enterprise application in GCP while mitigating these risks by applying least-privilege access, enforcing MFA, securely managing secrets, and establishing logging and budget safeguards.

---

### Lab Objectives

- Exercise 1: Set up a GCP project and IAM access controls  
- Exercise 2: Deploy and register the enterprise application  
- Exercise 3: Enforce MFA and IAM policies for access control  
- Exercise 4: Secure secrets using Secret Manager

---

## Exercise 1: Environment Setup and IAM Controls  
**Estimated Timing**: 15 minutes

### Task 1: Create a GCP Project

1. Go to the [Google Cloud Console](https://console.cloud.google.com).
2. From the top navigation, click on the project selector and choose **New Project**.
3. Enter:
   - **Project name**: `cp-enterprise-app`
   - **Billing account**: Select an active billing account
   - **Organization/folder**: Choose appropriately if available
4. Click **Create** and wait for initialization

### Task 2: Create a Service Account for App Ownership

1. Navigate to **IAM & Admin > Service Accounts**
2. Click **+ Create Service Account**
3. Fill in:
   - **Name**: `app-owner`
   - **ID**: auto-generated
   - **Description**: `Service account for application ownership`
4. Assign the **Editor** role (can be more restrictive based on use case)
5. Complete creation and save the credentials if necessary

---

## Exercise 2: Deploy and Register the Enterprise Application  
**Estimated Timing**: 20 minutes

1. Go to **App Engine** and click **Create Application**
2. Choose:
   - **Region**: `us-east1`
   - **Runtime**: e.g., Python, Node.js, Java, etc.
3. Deploy sample code using Google Cloud Shell or Cloud Console upload
4. Example (via Cloud Shell):

```bash
gcloud app create --region=us-east1
gcloud app deploy
```

5. Once deployed, note the application URL

---

## Exercise 3: Enforce MFA and IAM Policy  
**Estimated Timing**: 15 minutes

### Task 1: Enforce MFA for Admin Accounts

1. Go to **Admin Console** (admin.google.com) → **Security > Authentication**
2. Enable **2-Step Verification** for organizational users
3. Under **Enforcement**, ensure all privileged users must enroll

### Task 2: Implement IAM Policy to Restrict Broad Permissions

1. Navigate to **IAM > Policies**
2. Ensure no user or service account has **Owner** role at project level unless absolutely required
3. Use **Predefined roles** instead of Basic roles:
   - e.g., **App Engine Admin**, **Log Viewer**, **Cloud SQL Client**

4. Apply **custom roles** for specific least-privilege needs when needed

---

## Exercise 4: Secure Secrets with Secret Manager  
**Estimated Timing**: 10 minutes

1. Navigate to **Security > Secret Manager**
2. Click **+ Create Secret**
3. Fill out:
   - **Name**: `app-client-secret`
   - **Secret value**: Paste your credential or token
4. Set automatic **replication** to default region or configure as needed
5. Click **Create**
6. To access from your app:
   - Use the GCP SDK (`gcloud secrets versions access latest --secret=app-client-secret`)
   - Or add `Secret Manager Accessor` role to the App Engine service account

---

## Summary of Key Security Practices

- **IAM**: Apply minimal access policies and avoid basic roles  
- **MFA**: Enforced for all admin-level users  
- **Secrets**: Stored securely in **Secret Manager**, never in source code  
- **Audit Logging**: Enabled by default via Cloud Audit Logs  
- **Project Tags & Labels**: Use to manage and monitor project costs

---

>**Side Task**: Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.