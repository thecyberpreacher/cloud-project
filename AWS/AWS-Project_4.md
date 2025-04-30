### **AWS: Implementing Zero-Trust Architecture**  
#### **CyberPreacher Edition**  

A mid-sized company is transitioning to a **Zero-Trust security model** in AWS. Previously, they relied on perimeter security but lacked **identity-based access controls**, making them vulnerable to unauthorized access and insider threats. Their new Zero-Trust strategy focuses on:  
- **Verifying identities** before granting access.  
- **Using IAM policies** to enforce security.  
- **Monitoring and auditing access attempts**.  
- **Protecting workloads** with network segmentation and access control policies.  

> **For all resources in this lab, we are using the AWS US-East-1 region.**  

---

## **Lab Objectives**  
In this AWS lab, you will complete the following exercises:  
1. **Exercise 1:** Enable AWS IAM Identity Center and Conditional Access.  
2. **Exercise 2:** Configure Multi-Factor Authentication (MFA) for users.  
3. **Exercise 3:** Implement VPC Network Segmentation and Private Access.  
4. **Exercise 4:** Monitor and Audit Access Logs.  

---
### Pre-exercise

### **Bulk Import of Users**  
#### **Task 1: Bulk operations for creating users with a .csv file**  
1. Navigate to **AWS IAM Identity Center**.  
2. Under **Users**, select **Bulk Import**.  
3. Download the **CSV template** and populate it with user details.  
4. Modify the file to add users in bulk. Ensure fields like **username, display name, and initial password** are filled.  
5. Upload the CSV file and submit the bulk creation request.  
6. Create a new **IAM Group** named **Management** and add the imported users.  


### **Exercise 1: Configure Conditional Access Policies in AWS IAM Identity Center**  
#### **Estimated Timing: 15 minutes**  

#### **Task 1: Create Conditional Access Policies**  
1. Navigate to **AWS IAM Identity Center** in the **AWS Management Console**.  
2. Under **Access Management**, select **Policies**.  
3. Click **Create Policy**, name it **Zero-Trust Access Control**.  
4. Configure the following settings:  
   - **Users**: Assign to a specific IAM group (e.g., **Management**).  
   - **Conditions**:  
     - Restrict access based on **device type** (Windows, iOS).  
     - Enforce **sign-in risk levels** (High, Medium).  
   - **Actions**:  
     - Require **multi-factor authentication (MFA)**.  
5. Attach the policy to the **Management IAM Group**.  

---

### **Exercise 2: Configure Multi-Factor Authentication (MFA) for Users**  
#### **Estimated Timing: 10 minutes**  

#### **Task 1: Enforce MFA through IAM Policies**  
1. Navigate to **AWS IAM**.  
2. Under **Access Management**, select **Users**.  
3. Click on a user and go to **Security Credentials**.  
4. Enable **MFA** and require authentication via **AWS MFA App** or **hardware token**.  
5. Apply MFA enforcement to all users in the **Management IAM Group**.  

---

### **Exercise 3: Implement VPC Network Segmentation and Private Access**  
#### **Estimated Timing: 20 minutes**  

#### **Task 1: Configure AWS VPC Segmentation**  
- Open **AWS Management Console** and navigate to **Amazon VPC**.  
- Create a new **VPC** to segment workloads logically:  
  - **Public Subnet**: Allows external applications and necessary services.  
  - **Private Subnet**: Houses sensitive workloads like databases and internal services.  
- Use **VPC Peering** or **Transit Gateway** to control inter-VPC communication.  

#### **Task 2: Apply Security Groups and NACLs**  
- **Security Groups** act as virtual firewalls for instances:  
  - Allow only necessary inbound/outbound traffic.  
  - Restrict access based on IP ranges and ports.  
- **Network ACLs (NACLs)** provide additional subnet-level security:  
  - Define rules to block unauthorized traffic.  
  - Implement **deny-by-default** policies for Zero Trust enforcement.  

#### **Task 3: Implement AWS PrivateLink for Secure Access**  
- **AWS PrivateLink** ensures private connectivity to AWS services without exposing workloads to the internet.  
- Steps to implement:  
  - Create a **Private Endpoint** for services like **Amazon RDS, S3, or Lambda**.  
  - Configure **DNS resolution** to route traffic through private endpoints.  
  - Ensure applications use **private connections** instead of public internet access.  

---

### **Exercise 4: Monitor and Audit Access Logs**  
#### **Estimated Timing: 15 minutes**  

#### **Task 1: Enable AWS CloudTrail and AWS Config**  
1. Navigate to **AWS CloudTrail** and enable logging for:  
   - **Sign-in attempts**.  
   - **IAM policy changes**.  
   - **Network activity**.  
2. Use **AWS Config** to track security compliance.  

#### **Task 2: Review Security Logs Using AWS CloudWatch**  
1. Open **AWS CloudWatch** and run queries to detect anomalies:  
   ```plaintext
   SELECT * FROM cloudtrail_logs WHERE eventName = 'ConsoleLogin' AND riskLevel != 'None'
   ```
2. Investigate suspicious login attempts and adjust policies accordingly.  

---

### **Outcome**  
By following these steps, you successfully implemented a **Zero-Trust Architecture** in AWS, securing identity access and network traffic. The organization now has a robust **identity-based security model**, protecting against unauthorized access and insider threats.

>**Side Task**: Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.