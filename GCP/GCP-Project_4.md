### **GCP: Implementing Zero-Trust Architecture**  
#### **CyberPreacher Edition**  

A mid-sized company is transitioning to a **Zero-Trust security model** in Google Cloud (GCP). Previously, they relied on perimeter security but lacked **identity-based access controls**, making them vulnerable to unauthorized access and insider threats. Their new Zero-Trust strategy focuses on:  
- **Verifying identities** before granting access.  
- **Using IAM policies** to enforce security.  
- **Monitoring and auditing access attempts**.  
- **Protecting workloads** with network segmentation and access control policies.  

> **For all resources in this lab, we are using the GCP `us-east1` region.**  

---

## **Lab Objectives**  
In this GCP lab, you will complete the following exercises:  
1. **Exercise 1:** Enable Identity-Aware Proxy (IAP) and Conditional Access.  
2. **Exercise 2:** Configure Multi-Factor Authentication (MFA) for users.  
3. **Exercise 3:** Implement VPC Network Segmentation and Private Access.  
4. **Exercise 4:** Monitor and Audit Access Logs.  

---
## Pre-exercise

### **Bulk Import of Users**  
#### **Task 1: Bulk operations for creating users with a .csv file**  

1. Navigate to **Google Cloud Console** and open **IAM & Admin**.  

2. Under **Users**, select **Bulk Import**.  

3. Download the **CSV template** and populate it with user details.  

4. Modify the file to add users in bulk. Ensure fields like **username, display name, and initial password** are filled.  

5. Upload the CSV file and submit the bulk creation request.  

6. Create a new **IAM Group** named **Management** and add the imported users.  

---

### **Exercise 1: Configure Conditional Access Policies in GCP IAM**  
#### **Estimated Timing: 15 minutes**  

#### **Task 1: Create IAM Policies for Zero-Trust Access Control** 

1. Navigate to **Google Cloud Console** and open **IAM & Admin**.  

2. Under **IAM Policies**, click **Create Policy** and name it **Zero-Trust Access Control**.  

3. Configure the following settings:  
   - **Users**: Assign to a specific IAM group (e.g., **Management**).  
   - **Conditions**:  
     - Restrict access based on **device type** (Windows, iOS).  
     - Enforce **sign-in risk levels** (High, Medium).  
   - **Actions**:  
     - Require **multi-factor authentication (MFA)**.  

4. Attach the policy to the **Management IAM Group**.  

#### **Task 2: Enable Identity-Aware Proxy (IAP)**  

1. Navigate to **Security > Identity-Aware Proxy** in the **Google Cloud Console**.  

2. Enable **IAP** for applications that require Zero-Trust access.  

3. Configure access policies to enforce **context-aware authentication**.  

---

### **Exercise 2: Configure Multi-Factor Authentication (MFA) for Users**  
#### **Estimated Timing: 10 minutes**  

#### **Task 1: Enforce MFA through IAM Policies**  

1. Navigate to **Google Cloud Console** and open **IAM & Admin**.  

2. Under **Security Settings**, enable **Multi-Factor Authentication (MFA)**.  

3. Require authentication via **Google Authenticator** or **hardware security keys**.  

4. Apply MFA enforcement to all users in the **Management IAM Group**.  

---

### **Exercise 3: Implement VPC Network Segmentation and Private Access**  
#### **Estimated Timing: 20 minutes**  

#### **Task 1: Configure GCP VPC Segmentation**  

- Open **Google Cloud Console** and navigate to **VPC Networks**.  

- Create a new **VPC** to segment workloads logically:  
  - **Public Subnet**: Allows external applications and necessary services.  
  - **Private Subnet**: Houses sensitive workloads like databases and internal services.  

- Use **VPC Peering** or **Cloud Interconnect** to control inter-VPC communication.  

#### **Task 2: Apply Firewall Rules and IAM Policies**  

- **Firewall Rules** act as virtual firewalls for instances:  
  - Allow only necessary inbound/outbound traffic.  
  - Restrict access based on IP ranges and ports.  

- **IAM Policies** provide additional subnet-level security:  
  - Define rules to block unauthorized traffic.  
  - Implement **deny-by-default** policies for Zero Trust enforcement.  

#### **Task 3: Implement Private Service Connect for Secure Access**  

- **Private Service Connect** ensures private connectivity to GCP services without exposing workloads to the internet.  

- Steps to implement:  
  - Create a **Private Endpoint** for services like **Cloud SQL, Cloud Storage, or Cloud Functions**.  
  - Configure **DNS resolution** to route traffic through private endpoints.  
  - Ensure applications use **private connections** instead of public internet access.  

---

### **Exercise 4: Monitor and Audit Access Logs**  
#### **Estimated Timing: 15 minutes**  

#### **Task 1: Enable Cloud Logging and Cloud Audit Logs**  

1. Navigate to **Google Cloud Console** and open **Cloud Logging**.  

2. Enable logging for:  
   - **Sign-in attempts**.  
   - **IAM policy changes**.  
   - **Network activity**.  

3. Use **Cloud Audit Logs** to track security compliance.  

#### **Task 2: Review Security Logs Using Cloud Monitoring**  

1. Open **Cloud Monitoring** and run queries to detect anomalies:  
   ```plaintext
   SELECT * FROM audit_logs WHERE eventName = 'ConsoleLogin' AND riskLevel != 'None'
   ```

2. Investigate suspicious login attempts and adjust policies accordingly.  

---


### **Outcome**  
By following these steps, you successfully implemented a **Zero-Trust Architecture** in GCP, securing identity access and network traffic. The organization now has a robust **identity-based security model**, protecting against unauthorized access and insider threats.  

For further details, check out Google's official guidance on **Zero Trust architectures** [here](https://cloud.google.com/architecture/framework/security/implement-zero-trust) and [here](https://cloud.google.com/blog/products/identity-security/applying-zero-trust-to-user-access-and-production-services).