# Splunk Basics – HTTP Log Analysis

---

## 🎯 Objective

In this lab, you will:

* Learn how to ingest and analyze HTTP logs using Splunk.
* Detect client errors and server errors.
* Identify suspicious web activity.
* Investigate large file transfers and suspicious URI access attempts.

---

## 🖥️ Lab Setup

### Requirements

* ✅ Splunk installed and accessible via Splunk Web
* ✅ HTTP logs in JSON format (Zeek-style)
* ✅ Basic familiarity with Splunk Search Processing Language (SPL)

### Data Source

* **Log Type:** Zeek-style HTTP logs
* **Format:** JSON
* **File:** `synthetic_zeek_http.json`

Download the HTTP log file and store it in a location accessible to Splunk for ingestion.

---

## ⚙️ Steps to Upload HTTP Log into Splunk

1. Open Splunk Web.
2. Navigate to:

`Settings → Add Data`

3. Select **Upload**.
4. Choose the file:

`synthetic_zeek_http.json`

5. Configure the input settings:

### Source Type

`json`

### Index

* Use `main`, or
* Create a new index called:

`http_lab`

6. Complete the upload process.
7. Verify that the logs are successfully indexed.

---

## 🔍 Lab Tasks

Use Splunk SPL queries to investigate HTTP activity.

### ✅ Task 1: Find the Top 10 Endpoints Generating Web Traffic

Identify the IP addresses generating the most HTTP requests.

```spl
index=http_lab sourcetype="json"
| stats count by "id.orig_h"
| sort -count
| head 10
```

#### Explanation

* `id.orig_h` represents the client/source IP address.
* This query helps identify top web traffic sources.

---

### ✅ Task 2: Count the Number of Server Errors (5xx)

Find how many server-side errors occurred.

```spl
index=http_lab sourcetype="json" status_code>=500 status_code<600
| stats count as server_errors
```

#### Explanation

* HTTP status codes `500–599` represent server errors.
* Useful for detecting misconfigurations or backend failures.

---

### ✅ Task 3: Identify Suspicious User-Agents

Detect HTTP requests associated with automated tools or scripted attacks.

```spl
index=http_lab sourcetype="json"
user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-checker/1.0")
| stats count by user_agent
```

#### Explanation

These user agents may indicate:

| Tool | Purpose | | sqlmap | Automated SQL injection tool | | curl | Command line HTTP client | | python-requests | Automated scripts | | botnet-checker | Potential bot scanning |


### ✅ Task 4: Find Large File Transfers (>500 KB)

Identify HTTP responses transferring large amounts of data.

```spl
index=http_lab sourcetype="json"
resp_body_len>500000
| table ts "id.orig_h" "id.resp_h" uri resp_body_len
| sort -resp_body_len
```

#### Explanation

* `resp_body_len` shows response size in bytes.

Large transfers may indicate:

* Data exfiltration
* Large downloads
* Malicious payload delivery

---

## 📊 Skills Practiced

* Splunk log ingestion
* HTTP log analysis
* Writing SPL queries
* Identifying web traffic anomalies
* Detecting automated attack tools

---

## 🚀 Additional Practice

### Find Most Requested URIs

```spl
index=http_lab sourcetype="json"
| stats count by uri
| sort -count
```

### Identify Client Errors (4xx)

```spl
index=http_lab sourcetype="json" status_code>=400 status_code<500
| stats count by status_code
```

### Detect Rare URIs Accessed Only Once

```spl
index=http_lab sourcetype="json"
| stats count by uri
| where count=1
```

---

## 📚 References

* Splunk Documentation: https://docs.splunk.com

---

## 📁 Recommended Repository Structure

```text
splunk-http-log-analysis-lab/
│
├── README.md
├── logs/
│   └── synthetic_zeek_http.json
├── screenshots/
│   ├── top_endpoints.png
│   ├── server_errors.png
│   ├── suspicious_user_agents.png
│   └── large_transfers.png
└── queries/
    └── http_spl_queries.md
```
