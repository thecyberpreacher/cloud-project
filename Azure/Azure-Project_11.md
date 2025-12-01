---
montero Lab:
    title: '11 - Organization Case Study 4'
    difficulty: 'Hard'
---

# Project 11: Organization Case Study 4
## montero Edition  

## Summary


## Compamy Overview  

cyber Corporation is a consulting company in Montreal.

cyber recently acquired a Vancouver-based company named montero, Inc.

## Existing Environment. cyber Environment

The on-premises network of cyber contains an Active Directory Domain Services (AD DS) forest named cyber.com.

cyber has a Microsoft 365 E5 subscription. The subscription contains a verified domain that syncs with the cyber.com AD DS domain by
using Entra ID Connect.

cyber has an Azure Active Directory (Entra ID) tenant named cyber.com. The tenant has Security defaults disabled.

The tenant contains the users shown in the following table.

|Name|Role|
|---|---|
|User1|None|
|User2|None|
|User3|User administrator|
|User4|Privileged role administrator|
|User5|Identity Governance Administrator|

The tenant contains the groups shown in the following table.

|Name|Type|Membership type|Owner|Members|
|---|---|---|---|---|
|IT_Group1|Security|Assigned|None|All users in the IT department|
|cyberUsers|Security|Assigned|None|User1, User2|

## Existing Environment. montero Environment

montero has an AD DS forest named montero.com

Existing Environment. Problem Statements

cyber identifies the following issues:

- Multiple users in the sales department have up to five devices. The sales department users report that sometimes they must contact the support department to join their devices to the Entra ID tenant because they have reached their device limit.
- A recent security incident reveals that several users leaked their credentials, a suspicious browser was used for a sign-in, and resources were accessed from an anonymous IP address.
- When you attempt to assign the Device Administrators role to IT_Group1, the group does NOT appear in the selection list.
- Anyone in the organization can invite guest users, including other guests and non-administrators.
- The helpdesk spends too much time resetting user passwords.
- Users currently use only passwords for authentication.

## Requirements. Planned Changes -

Cyber plans to implement the following changes:

- Configure self-service password reset (SSPR).
- Configure multi-factor authentication (MFA) for all users.
- Configure an access review for an access package named Package1.
- Require admin approval for application access to organizational data.
- Sync the AD DS users and groups of montero.com with the Entra ID tenant.
- Ensure that only users that are assigned specific admin roles can invite guest users.
- Increase the maximum number of devices that can be joined or registered to Entra ID to 10.

## Requirements. Technical Requirements

Cyber identifies the following technical requirements:

- Users assigned the User administrator role must be able to request permission to use the role when needed for up to one year.

- Users must be prompted to register for MFA and provided with an option to bypass the registration for a grace period.

- Users must provide one authentication method to reset their password by using SSPR. Available methods must include:

    - Email
    - Phone
    - Security questions
    - The Microsoft Authenticator app
    - Trust relationships must NOT be established between the cyber.com and montero.com AD DS domains.
    - The principle of least privilege must be used.

## Soltion

> Recreate the Existing Environment before you proceed

### Exercise 1: Configure Device Settings
#### Task: Increase Device Limit to 10

1. **Navigate to Entra ID (Entra ID)**
    - Sign in to the [Azure Portal](https://portal.azure.com).
    - Search for and select **Microsoft Entra ID**.

2. **Open Device Settings**
    - In the left menu, under **Manage**, select **Devices**.
    - Select **Device settings**.

3. **Configure Limit**
    - Locate **Maximum number of devices per user**.
    - Change the value to **10**.
    - Click **Save**.

### Exercise 2: Configure External Collaboration
#### Task: Restrict Guest Invitations

1. **Navigate to External Identities**
    - In **Microsoft Entra ID**, select **External Identities** from the left menu.
    - Select **External collaboration settings**.

2. **Modify Guest Invite Settings**
    - Under **Guest invite settings**, select **Only users assigned to specific admin roles can invite guest users**.
    - Click **Save**.

### Exercise 3: Configure Identity Protection and MFA
#### Task 1: Configure MFA Registration Policy

1. **Navigate to Identity Protection**
    - Search for **Identity Protection** in the Azure Portal.

2. **Configure MFA Registration**
    - Under **Protection**, select **MFA registration policy**.
    - **Assignments**: Select **All users**.
    - **Controls**: Ensure **Require Entra ID MFA registration** is selected.
    - **Enforce Policy**: Toggle to **On**.
    - Click **Save**.

#### Task 2: Configure Risk Policies (Remediate Security Risks)

1. **User Risk Policy**
    - In **Identity Protection**, select **User risk policy**.
    - **Assignments**: Select **All users**.
    - **User risk**: Set to **High**.
    - **Controls**: Select **Block access** or **Allow access** > **Require password change**.
    - **Enforce Policy**: **On**.
    - Click **Save**.

2. **Sign-in Risk Policy**
    - Select **Sign-in risk policy**.
    - **Assignments**: Select **All users**.
    - **Sign-in risk**: Set to **Medium and above**.
    - **Controls**: Select **Allow access** > **Require multi-factor authentication**.
    - **Enforce Policy**: **On**.
    - Click **Save**.

### Exercise 4: Configure Self-Service Password Reset (SSPR)
#### Task: Enable SSPR and Configure Methods

1. **Navigate to Password Reset**
    - In **Microsoft Entra ID**, select **Password reset**.

2. **Enable SSPR**
    - In **Properties**, select **All** (or specific groups if testing).
    - Click **Save**.

3. **Configure Authentication Methods**
    - Select **Authentication methods**.
    - Set **Number of methods required to reset** to **1**.
    - Select the following methods:
        - **Email**
        - **Mobile phone**
        - **Security questions**
        - **Mobile app notification** (Microsoft Authenticator)
        - **Mobile app code** (Microsoft Authenticator)
    - Click **Save**.

### Exercise 5: Configure Application Consent
#### Task: Require Admin Approval for Apps

1. **Navigate to Enterprise Applications**
    - Search for **Enterprise applications** in the Azure Portal.

2. **Configure Consent Settings**
    - Under **Security**, select **Consent and permissions**.
    - Select **User consent settings**.
    - Select **Do not allow user consent**. (This forces admin approval).
    - Under **Admin consent settings**, ensure **Users can request admin consent to apps they are unable to consent to** is set to **Yes**.
    - Click **Save**.

### Exercise 6: Configure Privileged Identity Management (PIM)
#### Task: Configure User Administrator Role

1. **Navigate to PIM**
    - Search for **Privileged Identity Management**.
    - Select **Microsoft Entra roles**.

2. **Configure Role Settings**
    - Select **Roles**.
    - Search for **User Administrator**.
    - Click on the role, then select **Settings** (or "Role settings" in the top bar).
    - Click **Edit**.

3. **Set Assignment Rules**
    - Under **Assignment**, ensure **Allow permanent eligible assignment** is unchecked.
    - Set **Maximum duration** to **1 year**.
    - Under **Activation**, ensure **Require approval to activate** is selected.
    - Click **Update**.

4. **Assign Role**
    - Go back to **Roles** > **User Administrator**.
    - Click **Add assignments**.
    - Select **User3**.
    - Click **Next**.
    - Ensure **Assignment type** is **Eligible**.
    - Set the duration to end in **1 year**.
    - Click **Assign**.

### Exercise 7: Fix Group Role Assignment
#### Task: Recreate IT_Group1 for Role Assignment

*Note: Existing Security Groups cannot be modified to allow role assignment after creation. You must recreate the group.*

1. **Create New Group**
    - In **Microsoft Entra ID**, go to **Groups**.
    - Click **New group**.
    - **Group type**: Security.
    - **Group name**: `IT_Group1_New`.
    - **Entra ID roles can be assigned to the group**: Switch to **Yes**.
    - **Membership type**: Assigned.
    - Add members from the IT department.
    - Click **Create**.

2. **Assign Role**
    - Go to **Roles and administrators**.
    - Search for **Device Administrators**.
    - Click **Add assignments**.
    - Select `IT_Group1_New`.
    - Click **Add**.

### Exercise 8: Sync montero Environment
#### Task: Configure Entra ID Connect for montero

1. **Prepare montero Server**
    - Log in to a server in the `montero.com` forest.

2. **Install Entra ID Connect**
    - Download and install **Entra ID Connect**.
    - Select **Custom installation**.

3. **Connect to Entra ID**
    - Enter credentials for the `cyber.com` Entra ID tenant (Global Administrator).

4. **Connect to Directories**
    - Add the `montero.com` forest.
    - **Important**: Do NOT configure a trust between `cyber.com` (on-prem) and `montero.com`. Entra ID Connect can sync multiple untrusted forests to a single tenant.

5. **Configure Sync**
    - Proceed with default filtering or configure as needed.
    - Enable **Password Hash Synchronization** (recommended for "Users currently use only passwords" issue, to allow cloud auth).
    - Complete the installation.

### Exercise 9: Access Reviews
#### Task: Configure Access Review for Package1

1. **Navigate to Identity Governance**
    - Search for **Identity Governance**.

2. **Create Access Review**
    - Go to **Entitlement management** > **Access packages**.
    - Select **Package1**.
    - Select **Access reviews**.

> Note: Delete all resources once project is completed to prevent unnecessary charges.

> Side Task: Share screenshots on LinkedIn showing attack visualization and include hashtags **#cloudprojectwithcyberpreacher** and **#CPwCP**. Share your experience and learnings from this SOC lab.