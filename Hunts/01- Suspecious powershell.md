# Hunt 01 — Suspicious PowerShell Behavior

## 🎯 Hunting Hypothesis:
An attacker may abuse PowerShell to execute commands while using legitimate Windows tooling.
Potentially suspicious PowerShell activity may be identified through unusual command-line arguments, execution characteristics, and process relationships.

---

## 🔎 Data Source:
Data Source and Details
Endpoint Telemetry: Sysmon.
Event: Process Creation.
Sysmon Event ID: 1.
SIEM: Splunk Enterprise.
Primary Fields: Image, CommandLine, ParentImage, User.

---

## 🧠 Hunting Approach:
This hunt will follow a hypothesis-driven methodology rather than searching for a predefined malicious filename or known indicator.

The investigation will:
1. Identify PowerShell process execution in Sysmon telemetry.
2. Analyze command-line arguments.
3. Examine parent-child process relationships.
4. Establish normal PowerShell activity observed in the lab.
5. Generate controlled suspicious PowerShell behavior.
6. Compare the simulated behavior with the baseline.
7. Develop behavioral SPL detection logic.
8. Convert the validated detection into a Sigma rule.
9. Evaluate possible false positives.

---

## 📊 Initial Telemetry Exploration:
The first stage of the hunt focuses on identifying existing PowerShell execution within Sysmon Process Creation events.
Results and observations will be documented after analyzing the available telemetry.
