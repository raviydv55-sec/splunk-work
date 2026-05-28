# FTP Brute Force Detection using Splunk

## Overview

This project demonstrates the simulation and detection of an FTP brute force attack using Linux logs and Splunk SIEM.
The objective of this project was to understand how attackers perform brute force attacks against FTP services and how security analysts can detect and analyze those activities using log analysis and dashboards.

---

# Project Workflow

## 1. Port Discovery and Service Enumeration

The target machine was scanned to identify open ports and running services.

The FTP service was discovered during enumeration, which became the attack surface for the brute force simulation.

### Activities Performed

* Open port scanning
* FTP service detection
* Service identification

### Screenshots

Located inside:

```text
FTP_BRUTE_DETECTION/attacker_side/
```

Files:

* `port_discovery.png`
* `service_detectionftp.png`

---

# 2. FTP Brute Force Simulation using Hydra

After discovering the FTP service, a brute force attack was simulated using Hydra.

The attack attempted multiple username/password combinations against the FTP server.

### Tool Used

* Hydra

### Objective

* Generate repeated failed login attempts
* Produce realistic authentication logs
* Analyze brute force patterns in Splunk

### Screenshot

* `hydra_success.png`

---

# 3. Successful FTP Login

After brute force simulation, valid credentials were manually entered to generate a successful login event.

This helped create:

* FAIL LOGIN events
* OK LOGIN events

which are useful for SOC monitoring and correlation analysis.

### Screenshot

* `FTP_access.png`

---

# 4. Log Collection and Analysis

The FTP authentication logs were collected from:

```bash
/var/log/vsftpd.log
```

The logs contained:

* Client IP addresses
* Failed login attempts
* Successful login attempts
* Connection events
* Usernames
* Process IDs (PID)

### Screenshots

Located inside:

```text
FTP_BRUTE_DETECTION/log/
```

Files:

* `vsftpdlog1.png`
* `vsftpdlog2.png`
* `vsftpdlog3.png`
* `vsftpdlog_success_msg.png`

---

# 5. Splunk SIEM Integration

The collected FTP logs were uploaded into Splunk for analysis.

A custom index was created:

```text
ftp_brute_force_attack
```

Splunk SPL queries were then used to:

* Extract attacker IP addresses
* Detect failed login attempts
* Detect successful logins
* Analyze attack timelines
* Build visual dashboards

---

# SPL Queries Used

## Extract Login Status

```spl
index="ftp_brute_force_attack"
| rex "(?<status>OK|FAIL) LOGIN"
| stats count by status
```

---

## Extract Attacker IP

```spl
index="ftp_brute_force_attack"
| rex "::ffff:(?<ip>\d+\.\d+\.\d+\.\d+)"
| stats count by ip
```

---

## Timeline of Failed Logins

```spl
index="ftp_brute_force_attack"
| rex "(?<status>OK|FAIL) LOGIN"
| where status="FAIL"
| timechart count
```

---

# Dashboard Visualizations

The Splunk dashboard includes:

* Attacker IP detection
* Login success/failure analysis
* Attack timeline visualization
* Authentication monitoring

### Dashboard Screenshots

Located inside:

```text
FTP_BRUTE_DETECTION/Dashboard/
```

Files:

* `attackers_ip.png`
* `timeline_of_attack.png`

---

# Technologies Used

* Linux
* FTP (vsftpd)
* Hydra
* Splunk Enterprise
* SPL (Search Processing Language)

---

# Cybersecurity Concepts Demonstrated

* Port Scanning
* Service Enumeration
* FTP Authentication Monitoring
* Brute Force Attack Simulation
* Log Analysis
* SIEM Monitoring
* Detection Engineering
* Dashboard Visualization

---

# Educational Purpose

This project was created strictly for educational and defensive cybersecurity learning purposes in a controlled environment.

---

# Author

Rabi Bhushan Yadav


