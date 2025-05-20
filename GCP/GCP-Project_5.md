---
CyberPreacher Lab:  
    title: '05 - Secure Virtual Machines in GCP using Firewall Rules'  
    difficulty: 'Medium'  

---

# **Project 05: Secure Virtual Machines in GCP using Firewall Rules & Instance Groups**  
## **CyberPreacher Edition**  

### **Project Scenario**  
A company is migrating its infrastructure to GCP and needs strict security controls for its Compute Engine instances. Their challenges include:  

- Unauthorized access due to poorly configured firewall rules.  
- Difficulty managing security policies for multiple instances across different roles.  
- Inconsistent security enforcement across workloads.  

> All resources in this lab will be deployed in the **us-east1 region**.  

---

## **Lab Objectives**  
In this lab, you will complete the following exercises:  

- **Exercise 1:** Create a Compute Engine instance  
- **Exercise 2:** Configure Firewall Rules for security  
- **Exercise 3:** Implement Instance Groups for easier security management  

---

## **Exercise 1: Create a Compute Engine Instance in GCP**  

### **Estimated Time:** 15 minutes  

> **Note:** Ensure that you have a Google Cloud account with an active subscription ([Google Cloud Console](https://console.cloud.google.com)) or sign up for [Google Cloud Free Tier](https://cloud.google.com/free).  

### **Task 1: Create a Compute Engine Instance**  

1. Log in to the [Google Cloud Console](https://console.cloud.google.com).  
2. In the **Navigation Menu**, go to **Compute Engine** > **VM Instances**.  
3. Click **Create Instance** and configure the following:  

   |Setting|Value|  
   |---|---|  
   |Instance Name|**secure-vm**|  
   |Region|**us-east1**|  
   |Zone|**us-east1-b**|  
   |Machine Type|**n2-standard-2 (2 vCPUs, 8 GB RAM)**|  
   |Boot Disk|**Ubuntu Server 22.04 LTS**|  
   |Networking|**Create a new VPC and subnet**|  
   |External IP|**Ephemeral**|  
   |Firewall|**Select "Allow HTTPS and SSH"**|  

4. Click **Create** to deploy the VM.  

### **Task 2: Create a VM using CLI (Option 2)**  

Alternatively, you can use **gcloud CLI** to create the VM. Open Cloud Shell or your local terminal and run:

```shell
gcloud compute instances create secure-vm \
    --zone=us-east1-b \
    --machine-type=n2-standard-2 \
    --image-family=ubuntu-2204-lts \
    --image-project=ubuntu-os-cloud \
    --tags=webserver
```

Verify the instance status with:

```shell
gcloud compute instances list
```

---

## **Exercise 2: Configure Firewall Rules for Instance Security**  

### **Estimated Time:** 10 minutes  

> **Firewall rules in GCP control inbound and outbound traffic at the network level.**  

### **Task 1: Create a Firewall Rule**  

1. Navigate to **VPC Network** > **Firewall**.  
2. Click **Create Firewall Rule** and define the following:  

   |Setting|Value|  
   |---|---|  
   |Name|**secure-vm-fw**|  
   |Network|**Select your VPC**|  
   |Priority|**1000**|  
   |Direction|**Ingress**|  
   |Target Tags|**webserver**|  
   |Source Range|**0.0.0.0/0**|  
   |Allowed Protocols|**TCP: 22, 80, 443**|  

3. Click **Create** to apply the firewall rule.  

### **Task 2: Verify Firewall Rules**  

1. Navigate to **Compute Engine** > **VM Instances**.  
2. Click on your **secure-vm instance**.  
3. Go to the **Network details** tab and review the attached firewall rules.  

> **Result:** Firewall rules ensure controlled access to the Compute Engine instance.  

---

## **Exercise 3: Implement Instance Groups for Easier Management**  

### **Estimated Time:** 10 minutes  

> **Instance Groups allow dynamic security management for similar workloads in GCP.**  

### **Task 1: Create an Instance Group**  

1. Navigate to **Compute Engine** > **Instance Groups**.  
2. Click **Create Instance Group** and define:  

   |Setting|Value|  
   |---|---|  
   |Name|**WebServerGroup**|  
   |Region|**us-east1**|  
   |Instance Template|**Create new or use existing**|  
   |Autoscaling|**Enabled**|  
   |Target Tags|**webserver**|  

### **Task 2: Apply Firewall Rules using Instance Group Tags**  

1. Modify **secure-vm-fw** firewall rule to apply to instances in **WebServerGroup** using tags.  
2. Define **Ingress Rules**:  

   |Protocol|Port|Source|Target Tags|Action|  
   |---|---|---|---|---|  
   |HTTPS|**443**|**0.0.0.0/0**|**webserver**|Allow|  
   |SSH|**22**|**0.0.0.0/0**|**webserver**|Allow|  

3. Click **Save** to enforce the firewall rules dynamically.  

> **Result:** Security policies are dynamically applied to instances in the Instance Group.

---

## **Summary of Security Enhancements**  

- **Firewall Rules**: Restrict unauthorized access at the network level.  
- **Instance Groups**: Enable dynamic grouping for easier policy management.  

> **Side Task:** Deploy another instance, assign it to the Instance Group, and verify traffic restrictions. Share findings on LinkedIn with hashtags **#CloudSecurityWithCyberPreacher #CPwCP**.  

> **Note:** Remember to delete resources after the lab to avoid unnecessary costs (terminate instances and remove firewall rules).  