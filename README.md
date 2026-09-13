# 🛡️ Threat Detection & Hunting Lab
A hands-on cybersecurity project focused on developing practical skills in **Threat Detection, Detection Engineering, and Threat Hunting** using Windows telemetry, Sysmon, Splunk, Sigma, and MITRE ATT&CK. This project builds upon my SOC monitoring experience by moving from known-indicator searches toward behavioral detection and hypothesis-driven threat hunting.

## 🎯 Objectives:
- Collect endpoint and authentication telemetry
- Develop behavioral detections using Splunk SPL
- Perform hypothesis-driven threat hunts
- Analyze process, command-line, file, registry, network, and authentication activity
- Develop portable detection rules using Sigma
- Investigate false positives
- Correlate multiple security events
- Map validated behaviors to MITRE ATT&CK
- Document investigation findings and detection logic

## 🧪 Lab Architecture:
        Controlled Activity / Simulation
                    |
                    v
            Windows Endpoint
                    |
          +---------+---------+
          |                   |
        Sysmon          Security Logs
          |                   |
          +---------+---------+
                    |
                    v
             Splunk Enterprise
                    |
          +---------+---------+
          |                   |
     Threat Hunting      SPL Detection
                              |
                              v
                         Sigma Rules

## 🛠️ Technologies:
| Technology | Purpose |
| Windows | Monitored endpoint |
| Sysmon | Endpoint telemetry |
| Windows Security Logs | Authentication telemetry |
| Splunk Enterprise | SIEM, investigation, and hunting |
| SPL | Detection and hunting queries |
| Sigma | Detection-as-code |
| MITRE ATT&CK | Behavior and technique mapping |
| Kali Linux | Controlled adversary simulation |

## 🔎 Planned Threat Hunts:
### Hunt 01 — Suspicious PowerShell Behavior
Hunt for unusual PowerShell execution based on command-line characteristics and process telemetry.

### Hunt 02 — Suspicious Parent-Child Processes
Investigate unusual process relationships and potentially suspicious execution chains.

### Hunt 03 — Living-off-the-Land Behavior
Hunt for potentially suspicious use of legitimate Windows utilities.

### Hunt 04 — Persistence Activity
Investigate endpoint telemetry for behaviors associated with persistence mechanisms.

### Hunt 05 — Authentication Anomalies
Analyze Windows authentication telemetry for unusual failed and successful logon patterns.

### Hunt 06 — Process and Network Correlation
Correlate process execution with network telemetry to investigate potentially suspicious activity.

## 🚨 Detection Engineering:
Selected hunting findings will be converted into:
- Splunk SPL detection rules
- Sigma detection rules
- Documented detection rationale
- False-positive considerations
- Investigation guidance

## 🧠 Hunting Methodology:
Hypothesis
    ↓
Identify Relevant Telemetry
    ↓
Develop Hunting Query
    ↓
Analyze Results
    ↓
Investigate Context
    ↓
Identify Suspicious Behavior
    ↓
Develop Detection
    ↓
Test & Tune
    ↓
Document Findings

## ⚠️ Disclaimer:
All activities documented in this repository are performed in a controlled lab environment on systems that I own and operate for educational and defensive-security purposes.
