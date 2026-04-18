
## 🎬 Watch Me Build This Lab!

[![Watch the video](link_to_thumbnail)](link_to_loom_video)

---

## 📋 Project Overview

This lab demonstrates how to programmatically create a ServiceNow 
incident ticket using PowerShell and the ServiceNow Table REST API. 
No manual portal work needed — the entire ticket is created via 
an automated script.

---

## 🛠️ Tools Used

- PowerShell 5.1
- ServiceNow Developer Instance
- ServiceNow Table REST API
- Basic Authentication (Base64)

---

## ✅ Prerequisites

- ServiceNow developer account at developer.servicenow.com
- PowerShell 5.1 or higher
- ServiceNow instance URL
- Admin credentials with itil and rest_service roles

---

## 📁 Project Steps

### Step 1: Set Up ServiceNow Developer Instance
- Created a free Personal Developer Instance at developer.servicenow.com
- Confirmed Admin credentials and verified itil and rest_service roles

### Step 2: Gather Incident Details
Before writing any code the following fields were prepared:

| Field | Value |
|---|---|
| short_description | Sales Dept HP LaserJet is offline |
| description | Main printer for sales is unresponsive |
| category | Hardware |
| subcategory | Printer |
| impact | 2 - Medium |
| urgency | 2 - Medium |

### Step 3: Encode Credentials for Basic Authentication
```powershell
$pair = "${User}:${Pass}"
$bytes = [System.Text.Encoding]::UTF8.GetBytes($pair)
$encoded = [Convert]::ToBase64String($bytes)
```

### Step 4: Build Request Headers
```powershell
$Headers = @{
    "Accept"        = "application/json"
    "Content-Type"  = "application/json"
    "Authorization" = "Basic $encoded"
}
```

### Step 5: Define Incident Body
```powershell
$Body = @{
    short_description = "Sales Dept HP LaserJet is offline"
    description       = "Main printer for sales is unresponsive"
    category          = "Hardware"
    subcategory       = "Printer"
    impact            = "2"
    urgency           = "2"
} | ConvertTo-Json
```

### Step 6: Fire the API Call
```powershell
$Response = Invoke-RestMethod `
    -Uri "https://dev384027.service-now.com/api/now/table/incident" `
    -Method Post `
    -Headers $Headers `
    -Body $Body
```

### Step 7: Read the Response
```powershell
$Response.result | Format-List
```

---

## 📸 Screenshots

### PowerShell Response
<img width="2102" height="1196" alt="Screenshot 2026-04-17 152105" src="https://github.com/user-attachments/assets/3786fa7f-e5c8-4588-95ac-59be0e43777c" />



### Incident List View
<img width="2102" height="1196" alt="Screenshot 2026-04-17 152105" src="https://github.com/user-attachments/assets/bfa0c552-e13a-4df2-ab80-de7ee31386d3" />

### Incident Record Verified
<img width="2524" height="911" alt="Screenshot 2026-04-17 151913" src="https://github.com/user-attachments/assets/d7a417bd-2b09-4c30-a664-ca6b1a057945" />

---

## 🚨 Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| 401 Unauthorized | Wrong credentials or incorrect casing | Verify username casing — Admin vs admin matters |
| 403 Forbidden | Missing itil or rest_service role | Ask admin to assign correct roles |
| 400 Bad Request | Malformed JSON | Check $Body block for missing commas or quotes |

---

## 🔐 Security Notes

- Never hardcode credentials in scripts
- Use Azure Key Vault in production environments
- Use dedicated service accounts with minimum required roles
- Switch to OAuth 2.0 for production integrations
