SSH Log Analysis Using Splunk

Project Overview

This project focuses on analyzing SSH authentication logs using Splunk SIEM to detect:

 Successful SSH logins

 Failed login attempts

 Multiple failed authentication attempts (brute-force indicators)

 Suspicious SSH connections without authentication

This project simulates real-world SOC analyst work, including log ingestion, detection, dashboards, and alert creation.

 Lab Environment

Splunk Enterprise / Free (local Ubuntu VM)

ssh_log.json dataset

Ubuntu Linux Machine

Web browser with Splunk access (http://localhost:8000
)

 Project Files

ssh_log.json → Raw log file

Screenshots → All tasks documented via screenshots

README.md → (This file)

 Lab Setup
Step 1: Log into Splunk

Navigate to:

http://localhost:8000

Step 2: Upload SSH Logs

Go to: Apps → Search & Reporting

Click Add Data → Upload File

Upload ssh_log.json

Set:

Sourcetype: _json

Index: ssh_logs1

Click Start Searching

✔ Fields Verified

After ingestion, Splunk should extract:

event_type

auth_success

auth_attempts

id.orig_h (source IP)

id.resp_h (destination host)

 Task 1 — Validate Log Ingestion
Search Command
index=ssh_logs1 | stats count by event_type

 What You Confirmed

Logs are parsed

Events correctly categorized

SSH activity types are visible



 Task 2 — Analyze Failed Login Attempts
List all failed attempts
index=ssh_logs1 event_type="Failed SSH Login"
| stats count by id.orig_h

 What You Found

Identified IPs causing failed SSH attempts

Highlighted suspicious behavior

Visualization: Bar Chart: Failed Login Attempts per Source IP



 Task 3 — Detect Brute-Force Attempts
Find multiple failed auth attempts
index=ssh_logs1 event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h

Create Splunk Alert

Condition:

More than 5 login attempts from same IP within 10 minutes

Steps You Performed

Saved search → Create Alert

Set trigger conditions

Scheduled search every 10 minutes

Configured email / UI notification



 Task 4 — Track Successful Logins
Search
index=ssh_logs1 event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h

 Purpose

Detect legitimate logins

Compare successful logins with prior failures

Identify possible account compromise

Dashboard Panel Created:
Top Source IPs with Successful Logins



 Task 5 — Suspicious SSH Connections (No Authentication)
Search
index=ssh_logs1 event_type="Connection Without Authentication"
| stats count by id.orig_h

Time-based Behavior Analysis
index=ssh_logs1 event_type="Connection Without Authentication"
| timechart count by id.orig_h
