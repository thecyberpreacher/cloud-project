# **Project: Deploy a Web Application with a WAF on GCP**  

## **Scenario**  
A small startup recently launched its first web application hosted on Google Cloud Platform (GCP). However, due to insufficient security measures, the app became vulnerable to attacks like SQL injection and cross-site scripting (XSS).  

Now, the startup aims to enhance its application security by configuring **Google Cloud Armor**, GCP’s Web Application Firewall (WAF), to safeguard against common web threats.  

---

## **Lab Objectives**  
In this project, you will:  
1. **Launch a Virtual Machine (VM) to Host a Web Application.**  
2. **Set Up a Load Balancer with Cloud Armor.**  
3. **Configure Cloud Armor WAF Rules to Protect Against SQL Injection and XSS.**  
4. **Simulate Attacks to Test WAF Rules.**  
5. **Monitor Cloud Armor Logs for Insights.**

---

## **Exercise 1: Launch a Virtual Machine to Host the Web Application**  

### **Objective**  
Deploy a simple web app hosted on a Compute Engine (GCE) instance.

### **Estimated Timing:** 10-15 minutes  

#### **Step 1: Create a Compute Engine Virtual Machine (VM)**  
1. Navigate to the [GCP Console](https://console.cloud.google.com).  
2. Go to **Compute Engine > VM Instances** and click **Create Instance**.  
3. Configure the instance:  
   - **Name:** `demo-web-app`.  
   - **Region:** Choose a preferred region (e.g., `us-central1`).  
   - **Machine Type:** `e2-micro` (Free Tier Eligible).  
   - **Boot Disk:** Use the **Debian GNU/Linux** image.  
4. Enable HTTP and HTTPS traffic under **Firewall Settings**.  
5. Click **Create** and wait for the VM to initialize.

#### **Step 2: Deploy the Web Application**  
1. Connect to the VM via SSH from the GCP Console.  
2. Install Apache Web Server:  
   ```bash
   sudo apt update
   sudo apt install apache2 -y
   sudo systemctl start apache2
   ```
3. Create a simple HTML page:  
   ```bash
   echo "<h1>Welcome to the GCP Demo Web App</h1>" | sudo tee /var/www/html/index.html
   ```
4. Verify the deployment by visiting the external IP of the VM in your browser (e.g., `http://<external-ip>`).

---

## **Exercise 2: Set Up a Load Balancer with Cloud Armor**  

### **Objective**  
Deploy a Global HTTP(S) Load Balancer to route traffic to your web app and integrate it with Cloud Armor for WAF capabilities.

### **Estimated Timing:** 20 minutes  

#### **Step 1: Create a Load Balancer**  
1. Go to **Network Services > Load Balancing** in the GCP Console.  
2. Click **Create Load Balancer** and choose **HTTP(S) Load Balancer**.  
3. Configure the frontend:  
   - **Name:** `demo-lb`.  
   - **Protocol:** HTTP.  
4. Configure the backend:  
   - **Instance Group:** Create an instance group with your VM from Exercise 1.  
   - **Health Check:** Configure a basic HTTP health check on port 80.  
5. Review and click **Create**.

#### **Step 2: Attach Cloud Armor to the Load Balancer**  
1. Navigate to **Cloud Armor Policies** under **Security**.  
2. Create a new policy with the name `demo-waf-policy`.  
3. Associate the Cloud Armor policy with your load balancer.

---

## **Exercise 3: Configure Cloud Armor WAF Rules**  

### **Objective**  
Protect the web application using Cloud Armor rules for SQL injection and XSS.

### **Estimated Timing:** 15 minutes  

#### **Step 1: Add Predefined WAF Rules**  
1. Open your `demo-waf-policy` in Cloud Armor.  
2. Add rules to block specific attacks:  
   - **SQL Injection:** Enable the predefined `sqli-statement-detection` rule.  
   - **XSS Attack:** Enable the predefined `xss-detection` rule.  
3. Set the rule action to **Deny** to block malicious requests.  
4. Save your changes.

#### **Step 2: Enable Logging for Cloud Armor**  
1. Turn on logging for your Cloud Armor policy to capture details of blocked requests.

---

## **Exercise 4: Simulate Attacks to Test WAF Rules**  

### **Objective**  
Verify Cloud Armor’s effectiveness by simulating malicious requests.

### **Estimated Timing:** 20 minutes  

#### **Step 1: Simulate SQL Injection**  
1. Use **curl** or tools like **Postman** to send an HTTP request with an SQL injection payload:  
   ```bash
   curl -X POST http://<load-balancer-ip>/login \
   -d "username=' OR 1=1; --"
   ```
2. Verify that the request is blocked and returns a 403 Forbidden response.

#### **Step 2: Simulate Cross-Site Scripting (XSS)**  
1. Send a GET or POST request with an XSS payload:  
   ```bash
   curl -X GET http://<load-balancer-ip>/?search=<script>alert('XSS')</script>
   ```
2. Confirm that the WAF blocks the request.

---

## **Exercise 5: Monitor Cloud Armor Logs for Insights**  

### **Objective**  
Analyze Cloud Armor logs to review blocked threats and refine WAF rules.

### **Estimated Timing:** 15 minutes  

#### **Step 1: Enable Logging in Cloud Logging**  
1. Go to **Operations > Logging > Logs Explorer** in the GCP Console.  
2. Filter logs to view entries from Cloud Armor.  

#### **Step 2: Analyze the Logs**  
1. Use filters to identify blocked requests and examine details like:  
   - IP addresses of attackers.  
   - Time and frequency of attacks.  
   - Specific rule violations.  

---

## **Outcome**  
By completing this project, you’ll have deployed a secure web application on GCP with Cloud Armor as your WAF to mitigate SQL injection and XSS attacks. This will help you understand how to utilize GCP services for enhanced web app security.
