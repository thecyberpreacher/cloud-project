---
CyberPreacher Lab:
    title: '06 - Secure Enterprise App Deployment'
    difficulty: 'Medium'
---


# Project 6: Secure Enterprise App Deployment
## CyberPreacher Edition


### Project Scenario

A rapidly growing fintech company is transitioning from a legacy on-premises system to cloud in order to modernize its infrastructure and scale operations. During an internal security review, the following issues were discovered:

- User accounts had excessive permissions, increasing the risk of insider threats.
- Secrets and API credentials were stored as plain text in configuration files.
- No multi-factor authentication (MFA) was enforced, leaving critical admin accounts vulnerable.
- There was no centralized logging, making it difficult to trace incidents or access patterns.
- The company exceeded monthly cloud spending due to lack of cost monitoring.

*Your task*: As part of the cloud engineering team, you will be responsible for deploying a secure enterprise application in AWS that resolves these risks by enforcing strict IAM practices, encrypting secrets, integrating monitoring, and setting up budget alerts.

### Lab Objectives

- Exercise 1: Create a dedicated environment and IAM users.
- Exercise 2: Deploy and secure the enterprise application.
- Exercise 3: Enforce MFA and conditional access using IAM policies.
- Exercise 4: Store application secrets in AWS Secrets Manager.

---

## Exercise 1: Environment and IAM Access Controls  
**Estimated Timing**: 15 minutes

### Task 1: Create a Project-specific Resource Group (Tag-based)

1. Log in to the [AWS Management Console](https://console.aws.amazon.com/).
2. Navigate to the **Resource Groups** drop-down (top navigation bar).
3. Click **Tag Editor > Manage Tags** and create a tag:
   - *Key:* `Project`
   - *Value:* `CPApp`
4. Use this tag for all project resources moving forward for management and cleanup.

### Task 2: Create an IAM User and Assign Admin Role

1. Go to **IAM > Users > Add Users**
2. Fill in:
   - *User name:* `jane.admin`
   - *Access type:* Select **Programmatic access** and **AWS Management Console access**
   - *Console password:* Auto-generated or custom
3. In **Permissions**, select **Attach policies directly**
4. Search for and attach **AdministratorAccess**
5. Click **Next** and create the user
6. Save the temporary credentials for first login

---

## Exercise 2: Deploy a Web Application and Configure Permissions  
**Estimated Timing**: 20 minutes

1. In the AWS Console, navigate to **Elastic Beanstalk > Create a new application**
2. Fill in:
   - *Application name:* `EnterpriseApp`
   - *Platform:* Choose **Python/Node.js/Java** based on your app
   - *Environment:* Web Server Environment
3. Upload your app code or sample project
4. Click **Create environment**

Once deployed:

1. Go to **IAM > Roles**
2. Assign permissions to the Elastic Beanstalk instance profile:
   - *Policy:* `AmazonSSMFullAccess`, `AmazonS3ReadOnlyAccess`

---

## Exercise 3: Enable MFA and IAM Policy Conditions  
**Estimated Timing**: 15 minutes

1. In **IAM > Users**, select `jane.admin`
2. On the **Security credentials** tab, choose **Assign MFA device**
3. Select **Virtual MFA device** (use Google Authenticator or similar)
4. Follow prompts to scan QR code and confirm codes

### Add a Conditional Access Policy

1. In **IAM > Policies**, click **Create Policy**
2. Go to **JSON** tab and paste:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "BoolIfExists": {
        "aws:MultiFactorAuthPresent": "false"
      }
    }
  }
}
```

3. Attach this policy to all privileged users. It enforces MFA for all operations.

---

## Exercise 4: Secure Secrets with AWS Secrets Manager  
**Estimated Timing**: 10 minutes

1. Go to **Secrets Manager > Store a new secret**
2. Select:
   - *Secret type:* Other type of secrets
   - *Key/Value pair:*  
     - *Key:* `AppClientSecret`  
     - *Value:* Paste your application secret
3. Click **Next**, name the secret `EnterpriseAppSecret`
4. Store it in region `US East (N. Virginia)`
5. In your app, retrieve the secret securely via AWS SDK or using instance roles


## Summary of Key Security Practices

- **IAM**: Principle of least privilege enforced through roles and policies
- **MFA**: Mandatory for privileged users
- **Secrets**: Stored securely in Secrets Manager, not hardcoded
- **Monitoring**: Log ingestion via CloudWatch
- **Cost Control**: Budget alerts active to prevent overspending

 >**Side Task**: Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.