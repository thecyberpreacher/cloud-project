---
CyberPreacher Lab:
    title: '10 - Organization Case Study 3'
    difficulty: 'Hard'
---

# Project 10: Organization Case Study 3
## CyberPreacher Edition  

## Summary


## Compamy Overview  

CyberPreacher LTD, Ltd. is a consulting company that has a main oQce in Montreal and branch oQces in London and Seattle. CyberPreacher LTD has a partnership with a company named Fabrikam, Inc. Fabrikam has an Azure Active Directory (Azure AD) tenant named fabrikam.com

### Existing Environment

The on-premises network of CyberPreacher LTD contains an Active Directory domain named CyberPreacher LTD.com. The domain contains an organizational unit (OU) named CyberPreacher LTD_Resources. The CyberPreacher LTD_Resources OU contains all users and computers. The CyberPreacher LTD.com Active Directory domain contains the relevant users shown in the following table.

|Name|Office|Department|
|---|---|---|
|Admin1|Montreal|Helpdesk|
|User1|Montreal|HR|
|User2|Montreal|HR|
|User3|Montreal|HR|
|Admin2|London|Helpdesk|
|User4|London|Finance|
|User5|London|Sales|
|User6|London|Sales|
|Admin3|Seattle|Helpdesk|
|User7|Seattle|Sales|
|User8|Seattle|Sales|
|User9|Seattle|Sales|

CyberPreacher LTD also includes a marketing department that has users in each office.

#### Microsoft 365/Azure Environment

CyberPreacher LTD has an Azure AD tenant named CyberPreacher LTD.com that has the following associated licenses:
 • Microsoft Office 365 Enterprise E5
 • Enterprise Mobility + Security E5
 • Windows 10 Enterprise E3
 • Project Plan 

Azure AD Connect is conMgured between Azure AD and Active Directory Domain Services (AD DS). Only the CyberPreacher LTD_Resources OU is synced. Helpdesk administrators routinely use the Microsoft 365 admin center to manage user settings. User administrators currently use the Microsoft 365 admin center to manually assign licenses. All users have all licenses assigned besides the following exceptions:

User administrators currently use the Microsoft 365 admin center to manually assign licenses. All users have all licenses assigned besides the following exceptions:
 • The users in the London oQce have the Microsoft 365 Phone System license unassigned.
 • The users in the Seattle oQce have the Yammer Enterprise license unassigned.
Security defaults are disabled for CyberPreacher LTD.com.

CyberPreacher LTD uses Azure AD Privileged Identity Management (PIM) to protect administrative roles.

### Problem Statements

 CyberPreacher LTD identiMes the following issues:

 • Currently, all the helpdesk administrators can manage user licenses throughout the entire Microsoft 365 tenant.
 • The user administrators report that it is tedious to manually conMgure the different license requirements for each CyberPreacher LTD oQce.
 • The helpdesk administrators spend too much time provisioning internal and guest access to the required Microsoft 365 services and apps.
 • Currently, the helpdesk administrators can perform tasks by using the User administrator role without justifcation or approval.
 • When the Logs node is selected in Azure AD, an error message appears stating that Log Analytics integration is not enabled

### Planned Changes and Requirements

#### Planned Changes

CyberPreacher LTD plans to implement the following changes:

 • Implement self-service password reset (SSPR).
 • Analyze Azure audit activity logs by using Azure Monitor.
 • Simplify license allocation for new users added to the tenant.
 • Collaborate with the users at Fabrikam on a joint marketing campaign.
 • ConMgure the User administrator role to require justification and approval to activate.
 • Implement a custom line-of-business Azure web app named App1. App1 will be accessible from the internet and authenticated by using Azure
 AD accounts.
 • For new users in the marketing department, implement an automated approval workflow to provide access to a Microsoft SharePoint Online site,
 group, and app.

 CyberPreacher LTD plans to acquire a company named ADatum Corporation. One hundred new ADatum users will be created in an Active Directory OU named
 Adatum. The users will be located in London and Seattle.

#### Technical Requirements

 CyberPreacher LTD identiMes the following technical requirements:

 • All users must be synced from AD DS to the CyberPreacher LTD.com Azure AD tenant.
 • App1 must have a redirect URI pointed to https://cyberpreacher.com/auth-response.
 • License allocation for new users must be assigned automatically based on the location of the user.
 • Fabrikam users must have access to the marketing department’s SharePoint site for a maximum of 90 days.
 • Administrative actions performed in Azure AD must be audited. Audit logs must be retained for one year.
 • The helpdesk administrators must be able to manage licenses for only the users in their respective oQce.
 • Users must be forced to change their password if there is a probability that the users’ identity was compromised.

## Solution  

### 1. Deploy Existing Environment

### Task 1 - Bulk operations for creating users with a .csv file
1. In the Microsoft Entra ID menu, first open Identity, then select Users and then select All users.

2. On the **Users	All users** tile, select the Bulk operations drop-down arrow and then Bulk create.

3. Selecting Bulk create will open a new tile. This tile provides a Download link to a template file that you will edit to populate with your user information and upload to add the bulk creation of users.

4. Select Download to download the .csv file.

5. The .csv template provides you with the fields included with the user profile. This includes the required username, display name, and initial password. You can also complete optional fields, such as Department and Usage location, at this time. The following screenshot is an example of how you can complete the .csvfile:

- Bulk import using csv file entry

- You can modify this file to add users in bulk. Note that you do not need to fill out all the field. As per the sample data provide, you mainly need to add the name and username information.

6. A sample CSV has been provided in the Lab_Files folder – [PR10BulkUser.csv](../Lab_Files/PR10BulkUser.csv).

Open Notepad.
Inside the PC, select the START button and type Notepad.
Open the SC300BulkUser.csv file
Change the enter your domain name to the domain of your Azure lab environment.
Save the file.

7. On the Bulk create users dialog, select the file folder icon on step 3.

8. Path to the Allfiles/Labs/Lab1 folder and select SC300BulkUser.csv file.

9. Select Open.

10. You will be notified that the file uploaded successfully.  Choose Submit to add the users.

> After the users have been created, you will be prompted that the creation has succeeded. Close the Bulk create users tile and the new users will be populated in the list of **Users**

### Planned Changes

#### Task 1 - Configure Self-Service Password Reset (SSPR)
1. Navigate to Azure Portal > Azure AD > Password reset
2. Enable SSPR for selected groups or all users
3. Configure authentication methods and registration options
4. Set up notification preferences

#### Task 2 - Set Up Azure Monitor for Audit Logs
1. Go to Azure Portal > Monitor
2. Create a Log Analytics workspace
3. Enable Azure AD logs integration
4. Configure retention period to one year
5. Create custom queries for audit activities

#### Task 3 - Automate License Management
1. Navigate to Azure AD > Groups
2. Create dynamic groups based on office location
3. Configure group-based licensing
4. Assign appropriate license packages to each location group

#### Task 4 - Configure External Collaboration
1. Go to Azure AD > External Identities
2. Set up B2B collaboration settings
3. Create a security group for Fabrikam users
4. Configure time-bound access (90 days)
5. Share marketing SharePoint resources

#### Task 5 - Configure PIM for User Administrator Role
1. Access Privileged Identity Management
2. Select User Administrator role
3. Configure activation requirements:
    - Require justification
    - Require approval
    - Set activation duration

#### Task 6 - Deploy App1
1. Create new Azure Web App
2. Configure authentication using Azure AD
3. Set redirect URI to https://cyberpreacher.com/auth-response
4. Configure app roles and permissions
5. Test access and authentication

#### Task 7 - Set Up Marketing Department Workflow
1. Create SharePoint site for marketing
2. Configure Microsoft Flow automation:
    - Trigger on new marketing user creation
    - Add approval steps
    - Grant SharePoint, group, and app access
3. Test workflow with sample user

### Technical Requirements

1. **Configure AD DS Synchronization**
    - Install Azure AD Connect on domain controller
    - Enable sync for all OUs including Adatum
    - Configure filtering rules
    - Verify synchronization status

2. **Set Up App1 Authentication**
    - Navigate to App registrations in Azure AD
    - Configure authentication settings
    - Add redirect URI: https://cyberpreacher.com/auth-response
    - Set up token configuration

3. **Implement Automated License Management**
    - Create dynamic groups per location
    - Configure group-based licensing rules
    - Assign license packages based on office location
    - Test with sample users

4. **Configure External Access for Fabrikam**
    - Set up B2B collaboration
    - Create time-limited access policy (90 days)
    - Configure SharePoint site sharing
    - Set up access reviews

5. **Configure Azure AD Auditing**
    - Enable audit log collection
    - Set up Log Analytics workspace
    - Configure 1-year retention policy
    - Create monitoring alerts

6. **Implement Scoped License Management**
    - Create administrative units per office
    - Assign helpdesk admins to respective units
    - Configure role-based access control
    - Test license management restrictions

7. **Set Up Identity Protection**
    - Enable Azure AD Identity Protection
    - Configure risk-based policies
    - Set up forced password change triggers
    - Test compromised identity scenarios

> Note: Delete all resources once project is completed to prevent unnecessary charges.

> Side Task: Share screenshots on LinkedIn showing attack visualization and include hashtags **#cloudprojectwithcyberpreacher** and **#CPwCP**. Share your experience and learnings from this SOC lab.