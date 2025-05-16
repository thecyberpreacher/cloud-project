---
CyberPreacher Lab:  
    title: '05 - Secure Virtual Machines in Azure using NSG & ASG'  
    difficulty: 'Medium'  

---

# Project 05: Secure Virtual Machines in Azure using NSG & ASG  
## CyberPreacher Edition  

## Project scenario  

A company is migrating its infrastructure to Azure but wants to implement strict security controls for its virtual machines (VMs). Their challenges include:  

- Unauthorized access due to poorly configured firewall rules.  
- Difficulty managing network policies for multiple VMs across different roles.  
- Inconsistent application security enforcement across workloads.  

> For all the resources in this lab, we are using the **East US** region.  

## Lab objectives  

In this lab, you will complete the following exercises:  

- **Exercise 1:** Create a Virtual Machine in Azure  
- **Exercise 2:** Configure Network Security Groups (NSG) for VM Security  
- **Exercise 3:** Implement Application Security Groups (ASG)

## Instructions  

### **Exercise 1: Create a Virtual Machine in Azure**  

#### Estimated timing: 15 minutes  

>**Note**: Please ensure that you have created an Azure Account with active subscription credit ([Azure Portal](https://portal.azure.com)), or subscribe to [Azure Student Package](https://aka.ms/student-account).  

In this exercise, you will complete the following tasks:  

- **Task 1:** Create a Virtual Machine  
- **Task 2:** Configure VM Networking  
- **Task 3:** Validate VM Deployment  

#### Task 1: Create a Virtual Machine  

1. Start a browser session and sign in to [Azure Portal](https://portal.azure.com). 

2. In the **Search resources, services, and docs** text box, type **Virtual Machines** and press **Enter**. 

3. Click **Create** > **Azure Virtual Machine**.  

4. Configure the following settings:  

   |Setting|Value|  
   |---|---|  
   |Subscription|**choose-your-subscription**|
   |Resource Group|**CyberP-Project**|  
   |VM Name|**SecureVM1**|  
   |Region|**East US**|  
   |Availability|**No Availability**|
   |Security type|**Standard**|
   |Image|**Ubuntu Server 22.04 LTS**|  
   |Architecture|**x64**|
   |Size|**Standard_D2s_v3 - 2vcpus, 8 Gib**|
   |Authentication|**SSH Key (setup your usename)**|
   |Public inbound ports|**None**| 

4. Click **Networking**.  

5. Create a new **Virtual network**.

6. Configure the following settings:  

   |Setting|Value|  
   |---|---|  
   |Name|**secure-vnet**|
   |Address range|**192.168.0.0/16**|  
   |Subnet name|**subnet1**|  
   |subnet address|**192.168.1.0/24**| 

7. Click **OK**.

8. Click **Review + Create** and deploy the VM.  

### Task 2: Create a virtual machine using the CLI (option 2)

1. Use the icon (top right) to launch a Cloud Shell session. Alternately, navigate directly to https://shell.azure.com.

2. Be sure to select Bash. If necessary, configure the shell storage.

3. Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the [az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) command reference for all the parameters associated with creating a virtual machine.

```shell
 az vm create --name myCLIVM --resource-group CyberProject --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys
```
Once the command completes, use az vm show to verify your machine was created.

```shell
 az vm show --name  myCLIVM --resource-group CyberProject --show-details
 ```
Verify the powerState is VM Running.



### **Exercise 2: Configure Network Security Groups (NSG) for VM Security**  

#### Estimated timing: 10 minutes  

> **Network Security Groups (NSG) act as firewalls that control inbound and outbound traffic to a VM.**  

In this exercise, you will complete the following tasks:  

- **Task 1:** Create and Attach an NSG  
- **Task 2:** Configure NSG Rules  

#### Task 1: Create and Attach an NSG  

1. Navigate to **Azure Portal** > **Networking** > **Network Security Groups**.  
2. Click **Create NSG** and configure the following:  

   |Setting|Value|  
   |---|---|  
   |Name|**SecureVM-NSG**|  
   |Resource Group|**CyberP-Project**|  
   |Region|**East US**|  

3. Attach the NSG to the **VM Subnet or NIC**.  
4. Save the configuration.  

#### Task 2: Configure NSG Rules  

1. Open **SecureVM-NSG**.

2. On the left pane, click **Settings** then select **Inbound security rules**

3. Click **Add** then Define the following policies:  

   |Settings|Action|  
   |---|---|---|  
   |Source|Any|  
   |Source port ranges|*| 
   |Destination|Any|
   |Service|SSH| 
   |Action|Allow|  

> Result: NSG rules are applied, ensuring controlled access to the VM.  

4. Click **Add**.

5. On the left pane, click **Settings** then select **Outbound security rules**

6. Click **Add** then Define the following policies:  

   |Settings|Action|  
   |---|---|---|  
   |Source|Any|  
   |Source port ranges|*| 
   |Destination|Any|
   |Service|SSH| 
   |Action|Allow|  

> Result: NSG rules are applied, ensuring controlled access to the VM.  

7. Click **Add**.
>Since we have a Linux server, we need port 22 open for remote access.

#### Task 4: Associate NSG to virtual network created.

1. Open **SecureVM-NSG**.

2. On the left pane, Under **Settings**, click **Subnets**.

3. Click on **+ Associate** button.

4. Select **Secure-vnet**, and select **subnet1**.

5. Click **Associate**.

> This applies the rule to every VM in the subnet, so in production scenario, grouping related vm together using ASG first, then applying NSG on the ASG would be better. Learn how to create ASG below.

### **Exercise 3: Implement Application Security Groups (ASG)**  

#### Estimated timing: 10 minutes  

> **Application Security Groups (ASG) group VMs based on their role for easier security management.**  

In this exercise, you will complete the following tasks:  

- **Task 1:** Create Application Security Groups  
- **Task 2:** Assign VMs to ASG  
- **Task 3:** Apply NSG Rules using ASG  

#### Task 1: Create Application Security Groups  

1. Navigate to **Azure Portal** > **Networking** > **Application Security Groups**.  
2. Click **Create ASG** and define:  

   |Setting|Value|  
   |---|---|  
   |ASG Name|**WebServers**|  
   |Resource Group|**CyberP-Project**|  
   |Region|**East US**|  

#### Task 2: Assign VMs to ASG  

1. Go to **VM Networking Settings** > **Application Security Group**.  
2. Assign the **VM to WebServers ASG**.  

#### Task 3: Apply NSG Rules using ASG  

1. Modify **SecureVM-NSG** inbound/outbound rules to use ASGs.  
2. Click **Add** then Define the following policies:  

   |Settings|Action|  
   |---|---|---|  
   |Source|Any|  
   |Source port ranges|*| 
   |Destination|**Application security group (WebServers)**|
   |Service|HTTPS| 
   |Action|Allow|  

> Result: Security policies are dynamically applied based on ASG groups.

### **Summary of Security Enhancements**  

- **NSG**: Restricts unauthorized access at the network level.  
- **ASG**: Enables dynamic grouping for easier policy management. 

> **Side Task:** Deploy an additional VM, assign ASG, and verify traffic flow restrictions. Upload findings to LinkedIn with hashtags #CloudSecurityWithCyberPreacher #CPwCP.  

> **Note:** Remember to delete resources after the lab to avoid unnecessary costs (delete entire resource group to remove everything done).  