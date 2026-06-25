# Splunk Basics – SSH Log Analysis

## 🎯 Objective

In this lab, you will:

- Learn how to ingest and analyze SSH logs using Splunk
- Detect failed login attempts and successful connections
- Identify brute force attack patterns
- Investigate suspicious SSH authentication activity

---

# 🖥️ Lab Setup

## Requirements

✅ Splunk installed and accessible via Splunk Web

✅ SSH logs in JSON format (Zeek-style)

✅ Basic familiarity with Splunk Search Processing Language (SPL)

---

## Data Source

**Log Type:** Zeek SSH Logs

**Format:** JSON

**File:** ssh_logs.json

Download the SSH log file and store it in a location accessible to Splunk for ingestion.

---

# ⚙️ Steps to Upload SSH Log into Splunk

### Step 1: Open Splunk Web

Open Splunk Web Interface in your browser and log in.

### Step 2: Navigate to Add Data

Go to:

Settings → Add Data

Select:

Upload

### Step 3: Upload the Log File

Choose the file:

ssh_logs.json

This file contains SSH connection and authentication events generated in Zeek-style JSON format.

### Step 4: Configure Source Type

Set Source Type:

json

or create a custom source type:

ssh_logs

### Step 5: Configure Index

Use:

main

or create a dedicated index:

ssh_lab

### Step 6: Complete Upload

Finish the upload process and verify that the logs have been successfully indexed.

Run:

```spl
index=ssh_lab
```

If events appear, data ingestion was successful.

---

# 🔍 Lab Tasks

Use Splunk SPL queries to investigate SSH activity.

---

## ✅ Task 1: List Top 10 Endpoints with Failed SSH Login Attempts

Identify source IP addresses generating the highest number of failed SSH authentication attempts.

### SPL Query

```spl
index=ssh_lab sourcetype="json" auth_success=false
| stats count by "id.orig_h"
| sort -count
| head 10
```

### Explanation

- auth_success=false filters failed logins
- id.orig_h represents the source IP
- stats count counts failed attempts
- sort -count sorts results from highest to lowest
- head 10 displays the top 10 sources

### Security Insight

This query helps detect:

- Brute force attacks
- Repeated login attempts
- Suspicious external access attempts

---

## ✅ Task 2: Find the Total Number of SSH Connections

Determine the total number of SSH events present in the logs.

### SPL Query

```spl
index=ssh_lab sourcetype="json"
| stats count as total_ssh_connections
```

### Explanation

- stats count counts all SSH events
- total_ssh_connections renames the output field

### Security Insight

This query provides visibility into overall SSH activity within the environment.

---

## ✅ Task 3: Count All Event Types

Analyze the distribution of SSH event types.

### SPL Query

```spl
index=ssh_lab sourcetype="json"
| stats count by event_type
```

### Explanation

- event_type identifies the type of SSH activity
- stats count by event_type groups similar events together

### Common Event Types

| Event Type | Description |
|------------|-------------|
| successful | Successful SSH authentication |
| failed | Failed login attempt |
| no-auth | Connection attempt without authentication |
| multiple-failed | Multiple failed login attempts |

### Security Insight

This analysis helps identify:

- Authentication failures
- Successful logins
- Suspicious connection attempts
- Abnormal login behavior

---

# 📊 Skills Practiced

- Splunk Log Ingestion
- SSH Log Analysis
- Writing SPL Queries
- Authentication Monitoring
- Brute Force Detection
- Security Event Investigation

---

# 🚀 Additional Practice

### Find Successful SSH Logins

```spl
index=ssh_lab sourcetype="json" auth_success=true
| stats count by "id.orig_h"
| sort -count
```

### Detect Multiple Failed Attempts

```spl
index=ssh_lab sourcetype="json" event_type="multiple-failed"
| stats count by "id.orig_h"
```

### View Destination Servers

```spl
index=ssh_lab sourcetype="json"
| stats count by "id.resp_h"
| sort -count
```

### Detect Successful Logins After Failures

```spl
index=ssh_lab sourcetype="json"
| stats count by "id.orig_h" auth_success
```

---

# 🔐 Security Insights

Analyzing SSH logs can help detect:

### Brute Force Attacks

Large numbers of failed login attempts from a single source IP.

### Unauthorized Access

Unexpected successful logins from unknown systems.

### Suspicious Login Patterns

Examples:

- Many failed attempts followed by a successful login
- Logins at unusual times
- Repeated authentication failures
- Connections from unfamiliar hosts

Splunk enables real-time monitoring and threat detection through log analysis.

---

# 📚 References

Splunk Documentation:

https://docs.splunk.com

Zeek Documentation:

https://docs.zeek.org

---

# 👨‍💻 Author

**Vyankatesh Andil**

Cyber Security Enthusiast | SOC Analyst | CEH Certified | Splunk SIEM
