# Azure-Sentinel-Detection-Lab
A production-ready security monitoring lab built in **5 days** on Microsoft Azure. This project demonstrates **end-to-end detection engineering** – from log collection to automated incident response.
**What this lab proves:**
- Ability to build detection rules in KQL from scratch
- Implementation of MITRE ATT&CK-aligned alerting
- Automated incident response via email and Jira ticketing
- Practical Azure Sentinel engineering, not just theory

**What this lab proves:**
- Ability to build detection rules in KQL from scratch
- Implementation of MITRE ATT&CK-aligned alerting
- Automated incident response via email and Jira ticketing
- Practical Azure Sentinel engineering, not just theory

<img width="795" height="278" alt="image" src="https://github.com/user-attachments/assets/27402300-b218-422f-b548-f8a69e76b341" />


## 🎯 Detection Rules Implemented

| Rule | MITRE Technique | Detection Logic |
|------|----------------|-----------------|
| **Impossible Travel** | T1078 - Valid Accounts (T1078.001 - Default Accounts, T1078.002 - Domain Accounts, T1078.004 - Cloud Accounts) | Detects two successful logins from different countries within 30 minutes. Indicates possible account compromise or VPN abuse. |
| **Failed Logins Attempts** | T1110 (Brute Force) | 3+ failed password attempts in 15 minutes. Indicates brute force or credential stuffing attack. |
 
## 📊 Architecture
Entra ID Sign-in Logs → Microsoft Sentinel → Analytics Rules → Logic Apps → Email / Jira
- **Data Source**: Entra ID (Azure AD) sign-in logs
- **SIEM**: Microsoft Sentinel with custom KQL rules
- **Automation**: Logic Apps playbooks for email and Jira integration
- **Response**: Email notification + Jira ticket creation

---

## 🔧 Tech Stack

| Component | Technology |
|-----------|------------|
| SIEM | Microsoft Sentinel |
| Detection Language | KQL (Kusto Query Language) |
| Automation | Azure Logic Apps |
| Source Logs | Microsoft Entra ID |
| Ticketing | Atlassian Jira (API) |
| Notification | Office 365 Outlook |

## 🔧 Importing Playbooks

1. Go to Azure Logic Apps → **Import** → select `email_playbook.json`
2. Update connections (Outlook / Jira API)
3. Link to Sentinel analytics rules
