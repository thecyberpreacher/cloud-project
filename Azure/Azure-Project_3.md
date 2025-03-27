---
CyberPreacher Lab:
    title: '03 - Deploy a Web Application with a WAF'
    difficulty: 'Medium'
---

# Project 03: Deploy a Web Application with a WAF
## CyberPreacher Edition

## Project scenario

A small startup recently launched its first web application on the Azure cloud. However, they failed to implement proper security measures for the application. As a result:

- An attacker exploited a vulnerable endpoint on the application, exposing sensitive customer data.
- The company discovered malicious activities where attackers attempted SQL injections and cross-site scripting (XSS) attacks on their web app.
- Due to a lack of monitoring, they failed to detect these threats in time, resulting in reputational damage and operational downtime.

The company has now decided to enhance its web application security by configuring a Web Application Firewall (WAF) to protect against common web-based attacks, ensuring compliance with industry best practices and safeguarding customer trust.

> For all the resources in this lab, we are using the **East US** region.

## Lab Objectives  

In this lab, you will complete the following exercises:

1. **Exercise 1:** Create an Azure Web App.
2. **Exercise 2:** Configure the Web Application Firewall (WAF) on Azure Application Gateway.  
3. **Exercise 3:** Test the WAF with Simulated Attacks.
4. **Exercise 4:** Monitor and Audit WAF Logs.

---

## Instructions  

### **Exercise 1: Create an Azure Web App**  

#### Estimated Timing: 10 minutes  

> **Note:** Ensure you have an Azure account with an active subscription or use the Azure Student Package (https://aka.ms/student-account).  

In this exercise, you will deploy a simple Azure Web App.  

#### **Task 1: Create a Web App**  

1. Navigate to the [Azure portal](https://portal.azure.com).  
2. In the search bar, type **"App Services"** and select it from the results.  
3. Click **+ Add** and fill in the following details:

   |Setting|Value|
   |---|---|
   |Subscription|**Your Subscription**|
   |Resource Group|**CyberP-Project**|
   |Name|**demowebapp**|
   |Runtime Stack|**.NET 8**|
   |Region|**(US) East US**|
   |Pricing Plan|**Standard S1**|

4. Click **Review + Create**, then click **Create**.

> **Result:** You have deployed a simple Azure Web App. 

5. > You can continue as is, but optionally if you want to learn how to set up a web application, for more in-depth understanding, follow the [WebApp Deployment](WebApp_Deploy.md)  

### **Exercise 2: Configure the Web Application Firewall (WAF) on Azure Application Gateway**  

#### Estimated Timing: 15 minutes  

In this exercise, you will set up and configure a WAF.  

#### **Task 1: Deploy an Application Gateway**  

1. In the Azure portal, search for **Application Gateway** and select it.  
2. Click **+ Add** and configure the following:

   |Setting|Value|
   |---|---|
   |Resource Group|**CyberP-Project**|
   |Name|**demoAppGateway**|
   |Region|**(US) East US**|
   |Tier|**WAF V2**|
   |WAF Policy|**(new)newPolicy**|
   |Virtual network|**(new)WebVNET**|
   >  The (new) on each option shows that you need to create new item on that list with the name at the front.

3. Under **Frontend IP Configuration**, select **Public**.

4. Create new **Public IPv4 Address** using name **webappIP**.

5. Under **Backend Pool**, click **add**, select a name **backend** and change **Target type** to **App Services**,  add your **demowebapp** from Exercise 1.

6. Under **Configuration**, click **Add a routing rule**, input the following routing rule:

   |Setting|Value|
   |---|---|
   |Rule name|**route**|
   |Priority|**200**|
   |Listener|.|
   |Listener name|**newlistener**|
   |Backend targets|.|
   |Backend target|**backend**|

   |Backend settings|**(new)settings**|
   |Backend protocol|**HTTPS**|
   |Backend server certificate is issued by a well-known CA|**Yes**|

   >  Click +Add to continue

7. Click **+ Add**.  

8. Click **Review + Create**, then click **Create**.  

#### **Task 2: Configure WAF Rules**  

1. Once the Application Gateway is deployed, navigate to **WAF Policy** under **Settings**.  
2. Create a new WAF policy and enable **OWASP Core Rule Set (CRS)**.  
3. Set the rules to protect against **SQL Injection** and **Cross-Site Scripting (XSS)** attacks.  
4. Save and associate the policy with your Application Gateway.  

> **Result:** You have successfully deployed an Application Gateway with WAF and secured your web app.  

---

### **Exercise 3: Test the WAF with Simulated Attacks**  

#### Estimated Timing: 20 minutes  

In this exercise, you will test your WAF configuration to ensure it protects against SQL injection and XSS.  

#### **Task 1: Simulate Attacks**  

1. Open the firewall in a new tab (e.g https://172.210.107.17/)

>  Notice the webapp displays.

2. Add a **SQL Query** to the web url e.g (https://172.210.107.17/<script>alert('XSS')</script>)

3. Notice the error displayed

5. Now replace the payload with an XSS payload "<img src=x onerror=alert('XSS')>"

>  The reason for the error screen is because we didn't specify how the 503 block page would look like.

### **Exercise 4: Monitor and Audit WAF Logs**  

#### Estimated Timing: 15 minutes  

In this exercise, you will review WAF logs to analyze the detected attacks.  

#### **Task 1: Enable Diagnostic Logs**  

1. Navigate to your **Application Gateway** in the Azure portal.  
2. Under **Monitoring**, select **Diagnostics settings**.  
3. Add a new diagnostic setting and enable logging for **WAF Logs** and **Access Logs**.  
4. Save the logs to a Log Analytics Workspace for further analysis.  

#### **Task 2: Review Logs**  

1. Navigate to the **Log Analytics Workspace** in the Azure portal.  
2. Use KQL (Kusto Query Language) to filter and review WAF logs for attack attempts.  
3. Ensure no unauthorized access attempts are detected.  

> **Result:** You have successfully monitored and analyzed WAF logs.  

---

## Outcome  

> **Outcome:** By completing this project, you have deployed a secure web application with a Web Application Firewall (WAF) configured to protect against SQL injection and XSS attacks. This project demonstrates the importance of advanced web app security practices.