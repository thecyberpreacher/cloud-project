---
CyberPreacher Lab:  
    title: '05 - Secure Virtual Machines in AWS using Security Groups'  
    difficulty: 'Medium'  

---

# **Project 05: Secure Virtual Machines in AWS using Security Groups & Resource Groups**  
## **CyberPreacher Edition**  

### **Project Scenario**  
A company is migrating its infrastructure to AWS and needs strict security controls for its EC2 instances. Their challenges include:  

- Unauthorized access due to poorly configured security rules.  
- Difficulty managing security policies for multiple EC2 instances across different roles.  
- Inconsistent security enforcement across workloads.  

> All resources in this lab will be deployed in the **US East (N. Virginia) region**.  

---

## **Lab Objectives**  
In this lab, you will complete the following exercises:  

- **Exercise 1:** Create an EC2 instance  
- **Exercise 2:** Configure Security Groups for instance security  
- **Exercise 3:** Implement Resource Groups for easier management  

---

## **Exercise 1: Create an EC2 Instance in AWS**  

### **Estimated Time:** 15 minutes  

> **Note:** Ensure that you have created an AWS account with an active subscription ([AWS Console](https://aws.amazon.com/console)) or sign up for [AWS Free Tier](https://aws.amazon.com/free/).  

### **Task 1: Create an EC2 Instance**  

1. Log in to the [AWS Management Console](https://aws.amazon.com/console).  
2. In the **Search bar**, type **EC2** and press **Enter**.  
3. Click **Launch Instance** and configure the following:  

   |Setting|Value|  
   |---|---|  
   |AMI|**Ubuntu Server 22.04 LTS**|  
   |Instance Type|**t3.medium - 2 vCPUs, 8 GiB RAM**|  
   |Key Pair|**Create or use an existing SSH key**|  
   |Networking|**Select default VPC or create a new one**|  
   |Subnet|**Choose subnet within the VPC**|  
   |Auto-assign Public IP|**Enabled**|  
   |Security Group|**Create or select an SG (configured below)**|  
   |Storage|**Default (8 GiB, can be adjusted)**|  

4. Click **Launch** to deploy the instance.  
5. After deployment, navigate to **Instances** and verify the instance status.  

### **Task 2: Create an EC2 instance using CLI (Option 2)**  
Alternatively, you can use AWS CLI to create the instance. Open AWS CloudShell or your local terminal and run:

```shell
aws ec2 run-instances --image-id ami-xxxxxx --count 1 --instance-type t3.medium --key-name MyKeyPair --security-group-ids sg-xxxxxx --subnet-id subnet-xxxxxx
```

Once the instance is running, verify its status with:

```shell
aws ec2 describe-instances --instance-ids i-xxxxxxxx
```

---

## **Exercise 2: Configure Security Groups for Instance Security**  

### **Estimated Time:** 10 minutes  

> **Security Groups act as virtual firewalls that control inbound and outbound traffic to EC2 instances.**  

### **Task 1: Create a Security Group**  

1. In the **AWS Console**, navigate to **EC2** > **Security Groups**.  
2. Click **Create Security Group** and define the following:  

   |Setting|Value|  
   |---|---|  
   |Security Group Name|**SecureEC2-SG**|  
   |Description|**Security rules for EC2 instance**|  
   |VPC|**Select the default or created VPC**|  

### **Task 2: Configure Security Group Rules**  

1. Open **SecureEC2-SG**.  
2. Click **Inbound Rules**, then **Add Rule**:  

   |Protocol|Port|Source|Action|  
   |---|---|---|---|  
   |SSH|**22**|**My IP**|Allow|  
   |HTTP|**80**|**Anywhere**|Allow|  
   |HTTPS|**443**|**Anywhere**|Allow|  

3. Click **Save Rules**.  
4. Navigate to **Outbound Rules**, then **Add Rule**:  

   |Protocol|Port|Destination|Action|  
   |---|---|---|---|  
   |All Traffic|**All**|**Anywhere**|Allow|  

> **Result:** Security Groups ensure controlled access to the EC2 instance.  

5. Attach this Security Group to your EC2 instance by navigating to **Instances** > **Networking** > **Change Security Groups** > **Select SecureEC2-SG**.  

---

## **Exercise 3: Implement Resource Groups for Easier Management**  

### **Estimated Time:** 10 minutes  

> **AWS Resource Groups allow you to organize EC2 instances based on their function for better security and management.**  

### **Task 1: Create a Resource Group**  

1. Navigate to **AWS Console** > **Resource Groups** > **Create Group**.  
2. Define the following settings:  

   |Setting|Value|  
   |---|---|  
   |Group Name|**WebServerGroup**|  
   |Resource Type|**EC2 Instances**|  
   |Tagging Strategy|**Role=WebServer**|  

3. Click **Create**.  

### **Task 2: Assign EC2 Instances to Resource Group**  

1. Navigate to your EC2 instance.  
2. Add tags:  
   - **Key:** Role  
   - **Value:** WebServer  
3. The instance is now part of **WebServerGroup** for easier management.  

### **Task 3: Apply Security Group Rules using Resource Groups**  

1. Modify **SecureEC2-SG** to apply rules to EC2 instances in **WebServerGroup**.  
2. Define **Inbound Rules**:  

   |Protocol|Port|Source|Action|  
   |---|---|---|---|  
   |HTTPS|**443**|**Resource Group (WebServerGroup)**|Allow|  

3. Click **Save Rules**.  

> **Result:** Security policies are dynamically applied based on Resource Groups.

---

## **Summary of Security Enhancements**  

- **Security Groups**: Restrict unauthorized access at the network level.  
- **Resource Groups**: Enable dynamic grouping for easier policy management.  

> **Side Task:** Deploy another EC2 instance, assign Resource Group, and verify traffic flow restrictions. Share findings on LinkedIn with hashtags **#CloudSecurityWithCyberPreacher #CPwCP**.  

> **Note:** Remember to delete resources after the lab to avoid unnecessary costs (terminate EC2 instances and security groups).  