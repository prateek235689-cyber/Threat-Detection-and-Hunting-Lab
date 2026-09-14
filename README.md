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

## Objective
Identify unusual parent-child process relationships in Windows process creation telemetry and determine whether shell or scripting-engine execution patterns warrant further investigation.

## Hypothesis
An attacker may abuse legitimate applications or Windows processes to launch command shells or scripting engines. Unusual process ancestry can therefore provide useful behavioral indicators during threat hunting.

## Data Source
- Sysmon Event ID 1 — Process Creation
- Splunk Enterprise
- Windows endpoint telemetry

## Baseline Analysis
A seven-day baseline of Sysmon process creation telemetry was analyzed.

Initial results:
- 159,245 process creation events
- 383 unique parent-child process relationships

The dataset was heavily dominated by Splunk-related processes.

After excluding Splunk activity:
- 13,151 events remained
- 304 unique parent-child relationships remained

Filtering for relationships occurring five times or fewer produced:
- 2,383 events
- 140 rare parent-child relationships

Rare relationships were not automatically considered malicious. They were treated as candidates for further investigation.

## Shell and Script Interpreter Analysis

The hunt was narrowed to child processes involving:
- cmd.exe
- powershell.exe
- pwsh.exe
- wscript.exe
- cscript.exe

This produced:
- 75 process creation events
- 10 unique parent-child combinations

Several relationships were reviewed to understand whether their context appeared expected or required additional investigation.

## Baseline Candidate Investigation
One rare relationship observed was:

`svchost.exe → cmd.exe`
The associated command line was:
`"C:\WINDOWS\system32\cmd.exe" /d /c C:\WINDOWS\system32\hpatchmonTask.cmd`

The process executed as `NT AUTHORITY\SYSTEM`.

This event was treated as a contextual baseline finding rather than being classified as malicious. Parent-child relationships alone are insufficient to determine intent.

## Controlled Simulation
A benign controlled simulation was performed to intentionally create a PowerShell-to-command-shell relationship.

Command:
`powershell.exe -NoProfile -Command "Start-Process cmd.exe -ArgumentList '/c echo Hunt02-Test'"`

Observed process relationship:
`powershell.exe → cmd.exe`

Sysmon Event ID 1 successfully captured the process creation event.

## Behavioral Detection
A Splunk detection was created to identify PowerShell spawning Windows Command Shell.
The detection focuses on the behavioral relationship:

`ParentImage = powershell.exe`
and
`ProcessImage = cmd.exe`

The detection does not depend on the literal `Hunt02-Test` string, allowing it to identify the broader parent-child behavior.

Detection file:
`detections/splunk/suspicious-powershell-spawns-cmd.spl`

## Sigma Rule
The behavioral detection was translated into a Sigma rule:
`detections/sigma/suspicious-powershell-spawns-cmd.yml`

The rule detects:
`ParentImage endswith \powershell.exe`
AND
`Image endswith \cmd.exe`

Sigma CLI validation completed successfully with no rule, condition, or validation errors.
The rule was converted using the Splunk Windows processing pipeline.

Generated logic:
`ParentImage="*\\powershell.exe" Image="*\\cmd.exe"`

Because the lab ingests raw Sysmon XML, the required fields were extracted with Splunk `rex` before applying the Sigma-generated logic.
The adapted Sigma detection successfully identified the controlled PowerShell → cmd.exe event.

## False Positives and Limitations
PowerShell spawning cmd.exe is not inherently malicious.
Possible legitimate causes include:
- Administrative PowerShell scripts
- Automation
- Software installation
- Maintenance tasks
- Management tooling

Analysts should correlate the relationship with command-line arguments, user context, execution frequency, process ancestry, network activity, and other endpoint telemetry before determining severity.

## MITRE ATT&CK Mapping
- T1059.001 — PowerShell
- T1059.003 — Windows Command Shell

## Evidence
Screenshots are stored in:

`screenshots/hunt-02-parent-child/`

Evidence includes baseline analysis, rare relationship analysis, shell/interpreter investigation, controlled simulation, behavioral detection, and Sigma-generated detection validation.

## Conclusion:
This hunt demonstrated a behavior-driven approach to parent-child process analysis rather than assuming that rare activity is malicious.
Baseline analysis was used to reduce noise, candidate relationships were investigated in context, a controlled event was generated, and the resulting behavioral detection was implemented in both Splunk SPL and Sigma.

**Status: Completed**

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
