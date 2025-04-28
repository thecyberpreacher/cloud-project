# **Project: Deploy a Web Application with a WAF on AWS**  
## **Scenario**  
A small startup recently launched its first web application hosted on Amazon Web Services (AWS). Unfortunately, due to a lack of proper security measures, the application was vulnerable to attacks. As a result:  
- The application suffered SQL injection and Cross-Site Scripting (XSS) attacks.  
- Customer data was exposed, leading to a loss of trust and reputational damage.  
- The absence of monitoring delayed the detection and mitigation of these threats.  

The company now aims to enhance security by deploying a **Web Application Firewall (WAF)** on AWS to protect their web app against these attacks.

---

## **Lab Objectives**
In this project, you will:  
1. **Launch a Web Application on an EC2 Instance.**  
2. **Set Up an Application Load Balancer (ALB) with WAF.**  
3. **Configure AWS WAF Rules.**  
4. **Simulate and Test Attacks on the Web Application.**  
5. **Review AWS WAF Logs and Metrics.**

---

## **Exercise 1: Launch a Web Application on an EC2 Instance**  

### **Objective**  
Deploy a simple web application hosted on an Amazon EC2 instance.

### **Estimated Timing:** 10-15 minutes  

#### **Step 1: Launch an EC2 Instance**  
1. Log in to the [AWS Management Console](https://aws.amazon.com).  
2. Navigate to **EC2** under **Services** and click **Launch Instances**.  
3. Configure the instance:  
   - **Name:** `DemoWebApp`  
   - **AMI:** Amazon Linux 2 (Free Tier Eligible).  
   - **Instance Type:** t2.micro (Free Tier Eligible).  
   - **Key Pair:** Select an existing key pair or create a new one.  
   - **Network Settings:** Enable HTTP and SSH traffic.  
4. Click **Launch Instance** and wait for it to initialize.

#### **Step 2: Deploy a Sample Web Application**  
1. Connect to your EC2 instance via SSH using your terminal or a tool like PuTTY.  
   ```bash
   ssh -i <your-key.pem> ec2-user@<your-instance-public-ip>
   ```
2. Install Apache Web Server:  
   ```bash
   sudo yum update -y
   sudo yum install httpd -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```
3. Create a simple HTML page:  
   ```bash
   echo "<h1>Welcome to Demo Web App</h1>" | sudo tee /var/www/html/index.html
   ```
4. Verify your web app by visiting `http://<your-instance-public-ip>` in your browser.

---

## **Exercise 2: Set Up an Application Load Balancer (ALB) with WAF**  

### **Objective**  
Deploy an ALB in front of your EC2 instance and integrate AWS WAF.

### **Estimated Timing:** 20 minutes  

#### **Step 1: Create an Application Load Balancer**  
1. Go to the **EC2 Dashboard** and navigate to **Load Balancers**.  
2. Click **Create Load Balancer** and select **Application Load Balancer**.  
3. Configure the ALB:  
   - **Name:** `DemoALB`.  
   - **Scheme:** Internet-facing.  
   - **Listeners:** HTTP (port 80).  
   - **Availability Zones:** Select subnets in two or more AZs.  
4. Set up the target group:  
   - **Target Group Name:** `DemoTargetGroup`.  
   - **Target Type:** Instances.  
   - Add your EC2 instance to the target group.  
5. Review and create the ALB.

#### **Step 2: Attach WAF to the ALB**  
1. Navigate to **WAF & Shield** under AWS services.  
2. Click **Create Web ACL** and configure:  
   - **Name:** `DemoWAF`.  
   - **Region:** Same as your ALB.  
   - **Resource to Protect:** Choose your ALB.  
3. Save the Web ACL.

---

## **Exercise 3: Configure AWS WAF Rules**  

### **Objective**  
Set up WAF rules to block SQL injection and XSS attacks.

### **Estimated Timing:** 15 minutes  

#### **Step 1: Add AWS Managed Rules**  
1. In your Web ACL, click **Add Rules**.  
2. Add the **AWS-AWSManagedRulesSQLiRuleSet** to protect against SQL injections.  
3. Add the **AWS-AWSManagedRulesCommonRuleSet**, which includes XSS protection.  
4. Set the rules to **Block** action and save changes.

#### **Step 2: Test the Rules in Count Mode (Optional)**  
1. You can initially configure the rules in **Count Mode** to monitor how they behave without blocking traffic.  
2. Switch to **Block Mode** once verified.

---

## **Exercise 4: Simulate and Test Attacks**  

### **Objective**  
Verify that the WAF blocks SQL injection and XSS attempts.

### **Estimated Timing:** 15-20 minutes  

#### **Step 1: Simulate SQL Injection**  
1. Use tools like **Burp Suite** or **Postman** to send an HTTP POST request with an SQL payload:  
   ```
   http://<ALB-DNS-Name>/login  
   Payload: ' OR 1=1; --
   ```
2. Verify that the WAF blocks the request with a **403 Forbidden** response.

#### **Step 2: Simulate Cross-Site Scripting (XSS)**  
1. Send a GET or POST request with an XSS payload:  
   ```
   Payload: <script>alert('XSS')</script>
   ```
2. Confirm the WAF blocks the request.

---

## **Exercise 5: Review AWS WAF Logs and Metrics**  

### **Objective**  
Monitor the WAF to analyze blocked requests and fine-tune rules.

### **Estimated Timing:** 10 minutes  

#### **Step 1: Enable Logging**  
1. Navigate to your Web ACL and enable logging.  
2. Choose an **S3 bucket** or **CloudWatch Logs** to store logs.  

#### **Step 2: Analyze Logs**  
1. Use **AWS CloudWatch Insights** to filter and review WAF logs.  
2. Look for blocked requests and fine-tune rules as needed.



## **Outcome**  
By completing this project, you’ll have deployed a secure web application on AWS with WAF protection, configured to defend against common web-based attacks like SQL injection and XSS.

>**Side Task**: Once done, take a screenshot of the completed task and upload on LinkedIn including the Hashtag #cloudprojectwithcyberpreacher #CPwCP while sharing your experiences around the project.

 >**Note**: Ensure to delete every resources created during this project, to ensure cost management.