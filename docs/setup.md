# Deployment Guide

## Prerequisites

- Azure subscription (free trial with $200 credit)
- Global Administrator or Security Administrator role
- Jira account (for ticketing) – optional
- Outlook / Gmail account (for email) – optional

---

## Step 1: Create Log Analytics Workspace
<img width="945" height="215" alt="image" src="https://github.com/user-attachments/assets/97c44586-c72c-4324-8f1d-8b4c0099b198" />

1. Azure Portal → **Log Analytics workspaces** → **Create**
2. Name: `law-detection-lab`
3. Region: `North Europe` (or your preferred)
4. Pricing tier: `Pay-as-you-go`

---

## Step 2: Enable Microsoft Sentinel
<img width="945" height="212" alt="image" src="https://github.com/user-attachments/assets/b4aa2cd1-fb0f-4fda-ac4a-4384beac0fd7" />

1. In Azure Portal → search **Microsoft Sentinel**
2. Click **Create** → select `law-detection-lab`
3. Click **Add**

---

## Step 3: Connect Entra ID Logs
<img width="945" height="488" alt="image" src="https://github.com/user-attachments/assets/b7898aaf-7a5b-4e7f-98a2-7ec97ae6f29a" />
<img width="945" height="922" alt="image" src="https://github.com/user-attachments/assets/3d1976b6-3c79-4b78-ba47-689d373f8835" />
<img width="945" height="1135" alt="image" src="https://github.com/user-attachments/assets/bbf9cc7e-73a3-497e-b65d-41408cd2292c" />

1. In Sentinel → **Data connectors**
2. Search **Microsoft Entra ID**
3. Click **Open connector page**
4. Enable:
   - ✅ Sign-in logs
   - ✅ Audit logs
5. Click **Apply Changes**

**Note:** Logs may take 15–30 minutes to appear.

---

## Step 4: Create Analytics Rules
<img width="1020" height="373" alt="image" src="https://github.com/user-attachments/assets/5e174b6f-4e78-485b-97b0-077fb31f0950" />

### 4.1 Impossible Travel
<img width="945" height="426" alt="image" src="https://github.com/user-attachments/assets/3d8f863a-b175-456b-bf86-294cab38c711" />
<img width="945" height="565" alt="image" src="https://github.com/user-attachments/assets/00863e2d-50ca-426f-8d9f-e3a3c993c62d" />
<img width="945" height="329" alt="image" src="https://github.com/user-attachments/assets/30be0e11-98a0-4d5c-9130-d6cab5df1a03" />
<img width="945" height="378" alt="image" src="https://github.com/user-attachments/assets/d42925b2-c9b9-42db-a9a7-776a6fe42c71" />
<img width="945" height="458" alt="image" src="https://github.com/user-attachments/assets/96fa4df4-9684-4b02-8937-12c7a0e897a3" />
<img width="945" height="529" alt="image" src="https://github.com/user-attachments/assets/39306410-8efc-4407-aa29-d5c380a99b41" />




1. Sentinel → **Analytics** → **Create** → **Scheduled query rule**
2. Name: `Impossible Travel – Location Change in <30 min`
3. Paste KQL from `/rules/impossible_travel.kql`
4. Settings:
   - Run every: `5 minutes`
   - Lookup data: `Last 20 minutes`
   - Alert threshold: `> 0`
5. Entity mapping:
   - Account → Name → `UserPrincipalName`
   - IP → Address → `IPAddress`
6. Custom details:
   - `Country` → `Country`
   - `NextCountry` → `NextCountry`
   - `TimeDiff` → `TimeDiff`
7. Create rule

### 4.2 Failed Logins Attempts
<img width="945" height="451" alt="image" src="https://github.com/user-attachments/assets/8beffc2a-b4eb-4f46-8eaf-e621b71d1efb" />
<img width="945" height="485" alt="image" src="https://github.com/user-attachments/assets/0e35a617-50dc-452a-a37f-b7a323e4ec7c" />
<img width="945" height="460" alt="image" src="https://github.com/user-attachments/assets/e3f80277-e560-4241-b600-e0fda043956a" />
<img width="945" height="341" alt="image" src="https://github.com/user-attachments/assets/c1c63979-1e9c-477d-bc4d-bc13225dc12e" />
<img width="945" height="380" alt="image" src="https://github.com/user-attachments/assets/cc5ec502-8903-4b0c-a383-0192a034707c" />
<img width="945" height="268" alt="image" src="https://github.com/user-attachments/assets/0d0c4a78-f1c9-471d-88ee-9198ceb5277c" />
<img width="945" height="576" alt="image" src="https://github.com/user-attachments/assets/6b0e46a9-7d7d-4794-8700-c329cbb94ad6" />




1. Repeat above with `/rules/failed_logins_spike.kql`
2. Name: `Failed Logins Attempts – Invalid Password`
3. Custom details:
   - `FailedAttempts` → `FailedAttempts`
   - `StartTime` → `StartTime`
   - `EndTime` → `EndTime`

---

## Step 5: Configure Playbooks
<img width="1709" height="306" alt="image" src="https://github.com/user-attachments/assets/269e52a1-f0ec-4978-99f2-0aec1866db12" />


### 5.1 Email Playbook
<img width="945" height="488" alt="image" src="https://github.com/user-attachments/assets/6d55420a-7358-406d-80f4-0875983b5997" />
<img width="945" height="474" alt="image" src="https://github.com/user-attachments/assets/cee9f0f2-a255-4e62-9aaf-8d5a55b80dfb" />

1. Azure Portal → **Logic Apps** → **Create**
2. Name: `sentinel-email-playbook`
3. Region: same as workspace
4. Plan: `Consumption`
5. In designer:
   - Trigger: `When a Microsoft Sentinel alert is created`
   - Action: `Send an email (V2)` (Outlook/Gmail)
   - Body: Use expression `@{triggerBody()?['Entities']?[0]?['Name']}` for user
6. Save and link to analytics rule

### 5.2 Jira Playbook
<img width="945" height="485" alt="image" src="https://github.com/user-attachments/assets/b3372c67-f9c9-4d66-9e98-89e854e97e36" />
<img width="945" height="478" alt="image" src="https://github.com/user-attachments/assets/3fabc596-1fba-420f-b16e-ec74e9e6a2b5" />


1. Create Logic App: `sentinel-jira-playbook`
2. Trigger: `When a Microsoft Sentinel alert is created`
3. Action: `HTTP` (Jira API)
   - Method: `POST`
   - URI: `https://your-domain.atlassian.net/rest/api/3/issue`
   - Headers: Authorization + Content-Type
4. Body: JSON from `/playbooks/jira_playbook.json`

---

## Step 6: Test Your Setup

### Test 1: Impossible Travel

1. Log in to Azure Portal (without VPN)
2. Enable VPN to different country (e.g., Denamrk)
3. Log in again within 10 minutes
4. Wait 5 minutes → check **Incidents** in Sentinel

<img width="750" height="514" alt="image" src="https://github.com/user-attachments/assets/7c09284b-69c2-4e40-b116-cbc43abd9eb2" />

### Test 2: Failed Logins Attempts

1. Log out from your account
2. Enter wrong password 3+ times
3. Enter correct password (optional)
4. Wait 5 minutes → check **Incidents**

<img width="846" height="297" alt="image" src="https://github.com/user-attachments/assets/31d50ab1-b1fc-462e-bb02-8ab65c6a18b1" />

---

## Step 7: Verify Automation

1. In Sentinel → **Incidents** → select an incident
2. Check **Automation runs** tab
3. Confirm playbook executed successfully
4. Verify email received / Jira ticket created

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No logs in Sentinel | Wait 30 minutes, re-check data connector |
| Playbook not triggered | Verify automation rule linked to analytics rule |
| Empty email fields | Check entity mapping in analytics rule |
| Jira 401 error | Verify API token and permissions |
