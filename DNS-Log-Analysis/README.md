# Splunk Basics – DNS Log Analysis

## 🎯 Objective

In this lab, you will:

* Learn how to ingest and analyze DNS logs in Splunk.
* Understand how to extract useful information such as DNS query types, source hosts, and frequently queried domains.
* Practice building basic SPL (Search Processing Language) queries to investigate DNS activity.

---

# 🖥️ Lab Setup

## Requirements

* ✅ Splunk installed and accessible via Splunk Web
* ✅ Zeek DNS logs in JSON format
* ✅ Basic understanding of Splunk Search Processing Language (SPL)

## Data Source

**Log Type:** Zeek DNS Logs

**Format:** JSON

**File:** `dns.log`

Download the sample DNS log file and place it in a directory accessible to Splunk for ingestion.

---

# ⚙️ Steps to Upload DNS Log into Splunk

1. Open Splunk Web.

2. Navigate to:

```
Settings → Add Data
```

3. Select Upload.

4. Choose the file:

```
dns.log
```

5. Configure the input settings:

* **Source Type:** `json`

  *(or create a custom source type called `dns`)*

* **Index:**

  * Use `main`, or
  * Create a new index named:

```
dns_lab
```

6. Complete the upload process.

7. Verify that the logs are successfully indexed by running a search.

---

# 🔍 Lab Tasks

Use Splunk SPL queries to analyze DNS activity.

---

## ✅ Task 1: Identify the Most Frequently Queried Domain Names

Find the domains that appear most often in DNS queries.

```spl
index=dns_lab sourcetype="json"
| stats count by query
| sort -count
```

### Explanation

* `stats count by query` counts how many times each domain was queried.
* `sort -count` displays the most frequently queried domains first.

---

## ✅ Task 2: Find the Most Active User IPs Generating DNS Traffic

Identify which source IP addresses generate the most DNS requests.

```spl
index=dns_lab sourcetype="json"
| stats count by "id.orig_h"
| sort -count
```

### Explanation

* `id.orig_h` represents the originating host IP address in Zeek logs.
* This query helps identify top DNS traffic sources.

---

## ✅ Task 3: Breakdown of DNS Query Types

Analyze the distribution of DNS query types.

```spl
index=dns_lab sourcetype="json"
| stats count by qtype
```

### Common Query Types

Query Type Description A Maps a domain to an IPv4 address AAAA Maps a domain to an IPv6 address CNAME Canonical name (alias) for a domain PTR Reverse DNS lookup



---

# 📊 Skills Practiced

* Log ingestion in Splunk
* Understanding Zeek DNS logs
* Writing basic SPL queries
* DNS traffic investigation
* Identifying high-frequency domains and active hosts
