## **Deploying a demo Web Application in the Cloud Environment
### **Steps to Set Up a Simple Login Page**

#### **1. Choose a Backend Framework**  
Use a language and framework supported by Azure App Service, such as:  
   - **.NET Core**: Common for creating secure and robust web apps.  
   - **Node.js**: Lightweight and versatile for quick setups.  
   - **Python with Flask/Django**: Ideal for simple applications.  

---

#### **2. Create the Login Page (Frontend)**  
The login page can be created using HTML and styled with CSS. Here's a basic example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }
        form {
            display: inline-block;
            padding: 20px;
            border: 1px solid #ddd;
            background-color: #f9f9f9;
        }
    </style>
</head>
<body>
    <h1>Login</h1>
    <form action="/login" method="POST">
        <label for="username">Username:</label><br>
        <input type="text" id="username" name="username" required><br><br>
        <label for="password">Password:</label><br>
        <input type="password" id="password" name="password" required><br><br>
        <button type="submit">Login</button>
    </form>
</body>
</html>
```

---

#### **3. Add Backend Logic for Authentication**  
Implement a simple route to handle login credentials. For example, using Python with Flask:

```python
from flask import Flask, request, redirect, url_for, render_template

app = Flask(__name__)

# Hardcoded user credentials (for demonstration purposes)
USER_CREDENTIALS = {"admin": "password123"}

@app.route('/')
def home():
    return "Welcome to the Web App!"

@app.route('/login', methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form['username']
        password = request.form['password']
        if username in USER_CREDENTIALS and USER_CREDENTIALS[username] == password:
            return "Login Successful! Welcome, {}".format(username)
        else:
            return "Invalid Credentials. Please try again."
    return render_template("login.html")

if __name__ == "__main__":
    app.run(debug=True)
```

> **Note:** For production, avoid hardcoding credentials. Use secure methods like environment variables or external authentication services.

---

#### **4. Deploy the Updated Code**  
- Update your app with the new login page and backend logic.  
To deploy your code using the Azure CLI, follow these steps. This method assumes you’ve already written your app and want to deploy it to the Azure App Service created earlier (e.g., **demowebapp**). Here's a complete walkthrough:

---

### **Step 1: Install Azure CLI and Log In**
1. **Install Azure CLI**  
   - If you don’t have the Azure CLI installed, download and install it from the [Azure CLI Installation Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli).

2. **Log in to Azure**  
   - Open your terminal or command prompt and run:
     ```bash
     az login
     ```
   - Follow the prompts to authenticate with your Azure account. Once logged in, the CLI will return your subscription details.

---

### **Step 2: Prepare Your App for Deployment**
1. **Navigate to Your App’s Root Directory**  
   - Open the terminal and move to the folder containing your app’s code:
     ```bash
     cd /path/to/your/app
     ```

2. **Ensure Your App is Ready for Deployment**  
   - For example, if your app is a Python project, ensure it includes a `requirements.txt` file listing the dependencies. If it’s a Node.js project, ensure a `package.json` file exists (which is not applicable for this case scenario).

---

### **Step 3: Create a Deployment ZIP File (Optional)**  
- For certain types of apps, you may want to create a compressed ZIP file of your app for deployment:
  ```bash
  zip -r app.zip .
  ```

---

### **Step 4: Deploy the App Using Azure CLI**
1. **Set Variables**  
   - Replace `<app-name>` with your App Service name (e.g., `demowebapp`):
     ```bash
     AZURE_WEBAPP_NAME=<app-name>
     AZURE_RESOURCE_GROUP=CyberP-Project
     ```

2. **Deploy to Azure App Service**
   - If your code is in a ZIP file:
     ```bash
     az webapp deployment source config-zip \
       --resource-group $AZURE_RESOURCE_GROUP \
       --name $AZURE_WEBAPP_NAME \
       --src app.zip
     ```

   - If your code is in a local directory:
     ```bash
     az webapp up \
       --resource-group $AZURE_RESOURCE_GROUP \
       --name $AZURE_WEBAPP_NAME \
       --location "East US"
     ```

---

### **Step 5: Verify Deployment**
1. After deployment completes, the Azure CLI will output the URL for your web app. It will look something like this:
   ```
   https://<your-webapp-name>.azurewebsites.net
   ```
2. Open the URL in your browser to confirm the app is running successfully.

---

### **Step 6: Monitor Deployment (Optional)**
- To check the deployment logs for troubleshooting:
  ```bash
  az webapp log tail --name $AZURE_WEBAPP_NAME --resource-group $AZURE_RESOURCE_GROUP
  ```

---

That's it! You've successfully deployed your app using the Azure CLI. Let me know if you'd like help with any specific part of this process. 🚀
---