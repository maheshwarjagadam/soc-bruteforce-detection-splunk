# SMB Brute Force Attack Detection using Splunk

## Overview

This project simulates a brute-force (password spraying) attack from a Kali Linux machine against a Windows system and demonstrates how such activity can be detected using Splunk by analyzing Windows Security Event Logs.

The workflow follows a typical SOC process:
Attack Simulation → Log Generation → SIEM Ingestion → Detection Analysis

---

## Objectives

* Simulate brute-force authentication attempts over SMB
* Generate Windows Security Event ID 4625 (failed logons)
* Ingest logs into Splunk
* Detect attack patterns using SPL queries
* Identify attacker IP and targeted accounts

---

## Lab Setup

| Component | Details                    |
| --------- | -------------------------- |
| Attacker  | Kali Linux (192.168.20.11) |
| Target    | Windows 10 (192.168.20.10) |
| SIEM      | Splunk Enterprise          |
| Protocol  | SMB (Port 445)             |

---

## Tools Used

* Nmap
* Enum4linux
* CrackMapExec
* Splunk Enterprise
* Windows Event Viewer

---

## Attack Workflow

### 1. Reconnaissance

* Identified open ports using Nmap
* Confirmed SMB (port 445) availability

### 2. Enumeration

* Attempted user enumeration using enum4linux
* Identified possible accounts

### 3. Preparation

* Created user and password wordlists

### 4. Attack Execution

* Performed password spraying using CrackMapExec over SMB

### 5. Log Generation

* Windows recorded failed login attempts (Event ID 4625)

### 6. Detection

* Logs were ingested into Splunk
* Detection performed using SPL queries

---

## Detection Query

```spl
index=endpoint sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, Source_Network_Address
| sort - count
```

---

## Findings

* Multiple failed login attempts were detected
* Attacker IP identified: 192.168.20.11
* Multiple accounts were targeted
* Attack activity occurred within a short time window

---

## Screenshots

### Network Setup

![Attacker IP](./screenshots/01_attacker_ip_configuration.png)
![Target IP](./screenshots/02_target_ip_configuration.png)

### Reconnaissance

![Nmap SYN Scan](./screenshots/03_nmap_syn_scan.png)
![Service Enumeration](./screenshots/04_nmap_service_enumeration.png)
![SMB Port Open](./screenshots/05_smb_port_445_open.png)
![RDP Closed](./screenshots/06_rdp_port_3389_closed.png)

### Enumeration

![Enum4linux](./screenshots/07_enum4linux_user_enumeration.png)

### Preparation

![Users List](./screenshots/08_users_wordlist_created.png)
![Password List](./screenshots/09_password_wordlist_created.png)
![Windows Users](./screenshots/10_windows_actual_user_accounts.png)

### Attack Execution

![Brute Force](./screenshots/11_bruteforce_attack_execution_crackmapexec.png)

### Windows Logs

![Event Viewer](./screenshots/12_windows_eventlog_failed_logins_4625.png)

### Splunk Detection

![Raw Logs](./screenshots/13_splunk_raw_log_ingestion.png)
![Detection](./screenshots/14_splunk_bruteforce_detection_results.png)

---

## Skills Demonstrated

* Network scanning and service enumeration
* Brute-force attack simulation
* Windows Security log analysis
* SIEM (Splunk) log ingestion
* SPL query development
* Threat detection and analysis

---

## Future Improvements

* Implement real-time alerting in Splunk
* Build dashboards for visualization
* Extend to additional attack scenarios

---

## Disclaimer

This project was conducted in a controlled lab environment for educational purposes only.
