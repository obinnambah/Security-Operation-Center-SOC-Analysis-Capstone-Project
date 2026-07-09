# Heartland Memorial Hospital — SOC Analysis Capstone Project

A hands-on Security Operations Centre (SOC) project demonstrating how **pfSense**, 
**Wireshark**, and **Wazuh** can be integrated to monitor, detect, and respond to 
cyber threats in a healthcare environment. This project simulates real-world attack 
scenarios and implements centralized security monitoring to protect sensitive 
patient data and critical hospital infrastructure.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Tools and Technologies](#tools-and-technologies)
- [Network Environment Setup](#network-environment-setup)
- [Implementation Steps](#implementation-steps)
- [Step-by-Step Analysis](#step-by-step-analysis)
- [Key Findings](#key-findings)
- [Impact](#impact)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)
- [Author](#author)

---

## Project Overview

This capstone project focuses on the design and implementation of a **Security 
Operations Centre (SOC)** environment for **Heartland Memorial Hospital** — a 
healthcare organization that relies heavily on digital infrastructure for patient 
records management, communication, and internal operations.

Healthcare institutions are high-value targets for cybercriminals due to the 
sensitivity of patient data. This project demonstrates how open-source security 
tools can be combined to build an affordable, effective SOC solution capable of 
real-time threat detection and incident response.

---

## Problem Statement

Heartland Memorial faced the following cybersecurity risks:

- Unauthorized network access
- Malware and ransomware attacks
- Insider threats
- Weak visibility into network traffic
- Delayed incident detection and response

Without centralized monitoring, identifying and responding to threats becomes 
difficult and increases the risk of operational disruption and data breaches.

---

## Objectives

- Configure **pfSense** firewall for network security monitoring
- Analyze network traffic using **Wireshark**
- Deploy **Wazuh** for centralized log monitoring and threat detection
- Simulate network attacks and security incidents to evaluate SOC effectiveness

---

## Tools and Technologies

- **pfSense** — network firewall, gateway security, Snort IDS, GeoIP filtering
- **Wireshark** — deep packet inspection and traffic analysis
- **Wazuh** — SIEM platform, intrusion detection, and centralized log management
- **Snort IDS** — intrusion detection integrated with pfSense
- **MITRE ATT&CK Framework** — attacker behaviour mapping and threat intelligence
- **Ubuntu Linux** — Wazuh server operating system
- **VirtualBox / VMware** — virtualized SOC lab environment

---

## Network Environment Setup

A virtualized SOC lab environment was created consisting of:

- **pfSense firewall** — acting as the network gateway and security enforcement point
- **Wireshark monitoring workstation** — for live packet capture and traffic analysis
- **Wazuh server** — for centralized log collection, alerting, and threat detection

---

## Implementation Steps

### pfSense Configuration

1. Install and configure the **pfSense** firewall on the network gateway
2. Install and configure **Snort IDS** with GeoIP filtering on both LAN and WAN 
   interfaces
3. Enable firewall rules and logging to capture all inbound and outbound traffic
4. Configure **remote syslog forwarding** to send firewall logs to the Wazuh server
5. Create firewall aliases to block known malicious IP addresses 
   (e.g. `Phobos_ransomware` host group: `185.202.0.111`, `45.89.127.159`, 
   `194.165.16.4`)
6. Implement an **Anti-lockout Rule** on the LAN interface to protect against 
   malicious traffic lock-outs

### Wireshark Setup

1. Install **Wireshark** on the monitoring workstation
2. Begin live packet capture on the active network interface
3. Apply display filters (e.g. `http`, `dns`, `tcp`, `ssh`) to isolate traffic types
4. Capture and examine HTTP, TCP, DNS, and ICMP traffic patterns
5. Identify suspicious packet frequencies and port scanning behaviour

### Wazuh Deployment

1. Install the **Wazuh manager** and configure the web dashboard
2. Deploy **Wazuh agents** on all monitored endpoints and connect them to the manager
3. Configure centralized alert rules for authentication failures, firewall violations, 
   and malware signatures
4. Enable **MITRE ATT&CK** mapping within the Wazuh threat hunting dashboard
5. Enable **vulnerability detection** to scan for CVEs across connected endpoints

---

## Step-by-Step Analysis

### 4.1 Firewall Traffic Monitoring — pfSense

pfSense was configured to block unauthorized traffic, generate firewall logs, and 
detect suspicious IP addresses. The firewall successfully logged and blocked 
repeated connection attempts from unknown external IPs using the Snort IDS rule 
`Block snort2c hosts (1000000119)`.

**Observations:**
- Multiple blocked connection attempts were recorded on the LAN interface
- Unknown IP addresses attempted repeated access across multiple ports
- Phobos ransomware-associated IPs were successfully identified and blocked

---

### 4.2 Packet Analysis — Wireshark

Wireshark captured and analyzed the following traffic types:

- **HTTP traffic** — identified high-volume GET requests indicating potential 
  data exfiltration or malicious activity
- **ICMP requests** — used to detect ping sweeps and network reconnaissance
- **DNS traffic** — revealed unusual domain resolution queries linked to external 
  C2 infrastructure
- **SSH login attempts** — flagged as brute-force indicators

**Analysis Performed:**
- Identified unusual packet frequency patterns
- Detected port scanning behavior across multiple destination IPs
- Examined source and destination IP pairs for anomalies

**Findings:**
- High HTTP traffic volume indicated potential malicious activity on the network

---

### 4.3 Threat Detection — Wazuh SIEM

Wazuh generated real-time alerts related to:

- Authentication failures
- Firewall rule violations
- Malware signatures
- Suspicious user activities

**Vulnerability Detection Summary:**

| Severity | Count |
|---|---|
| Critical | 125 |
| High | 927 |
| Medium | 1,428 |
| Low | 69 |
| Pending Evaluation | 357 |

**Top CVEs Detected:**
- `CVE-2023-3326` — 16 instances
- `CVE-2026-27456` — 13 instances
- `CVE-2026-41079` — 11 instances

**MITRE ATT&CK Tactics Observed:**
- Defense Evasion
- Initial Access
- Persistence
- Privilege Escalation

---

## Key Findings

- **pfSense** effectively filtered unauthorized traffic and blocked malicious IPs 
  in real time
- **Wireshark** provided deep visibility into network activities, revealing 
  suspicious HTTP and DNS patterns
- **Wazuh** successfully centralized log monitoring, generated actionable alerts, 
  and mapped threats to the MITRE ATT&CK framework
- Simulated attacks — including port scanning and brute-force login attempts — 
  were detected in real time
- Security alerts significantly improved incident response speed

---

## Impact

### Positive Impacts

- Improved network visibility across all hospital systems
- Faster threat detection through real-time alerting
- Enhanced incident response with centralized log correlation
- Better compliance with cybersecurity standards
- Reduced overall security risk exposure

### Challenges Encountered

- False positive alerts required tuning and analyst review
- Large log volume created management complexity
- Continuous monitoring requires skilled SOC analysts
- Sustained resource commitment needed for 24/7 coverage

---

## Recommendations

1. Maintain **24/7 network monitoring** using the integrated SOC toolset for early 
   threat detection
2. Perform **regular updates** of pfSense, Wazuh, Snort rules, and the underlying 
   OS to patch known vulnerabilities
3. Conduct **staff training programs** to reduce susceptibility to phishing and 
   insider threats
4. Integrate **external threat intelligence feeds** with Wazuh for enhanced 
   detection capabilities
5. Perform **periodic vulnerability assessments and penetration testing** to 
   identify gaps proactively
6. Develop a robust **backup strategy and incident recovery plan** to minimize 
   downtime after an attack

---

## Conclusion

This capstone project successfully demonstrated how **pfSense**, **Wireshark**, and 
**Wazuh** can be integrated to build a functional and cost-effective Security 
Operations Centre for a medium-sized healthcare organization. The implementation 
improved network monitoring, accelerated threat detection, and strengthened 
incident response capabilities at Heartland Memorial Hospital.

The project highlighted that centralized security monitoring is essential for 
protecting sensitive patient data and critical organizational assets. Despite 
challenges such as false positives and log management complexity, the integrated 
SOC solution proved to be both efficient and affordable — making it a practical 
model for organizations without enterprise-level security budgets.

---

## Author

**Obinna Mbah**
SOC Analyst — 10Alytics
📍 Essen, Germany
📅 Submission Date: 29 May 2026
