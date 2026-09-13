# Sigma CLI Setup

## Overview:
Sigma was integrated into the Threat Detection & Hunting Lab to introduce **Detection-as-Code** and vendor-independent detection engineering.
While Splunk SPL is used to hunt and analyze telemetry inside Splunk, Sigma provides a standardized format for documenting detection logic that can be translated for supported SIEM platforms.

## Objective:
The objectives of this setup were to:
- Install Sigma CLI on the Windows analysis system
- Use an isolated Python virtual environment
- Prepare the lab for Sigma-based detection engineering
- Maintain detections as code inside GitHub
- Prepare Sigma rules for later conversion and testing with Splunk

## Environment
Component with Configuration:
Operating System: Windows.
Python version: Python 3.14.4.
pip version: 26.1.1.
Endpoint Telemetry: Sysmon.
Authentication Telemetry: Windows Security Logs.
SIEM: Splunk Enterprise.
Detection Framework: Sigma.
Detection Language: Splunk SPL / Sigma YAML.

## Sigma CLI Installation:
A dedicated directory was created for the threat hunting environment.

```command prompt:
mkdir C:\ThreatHuntingLab
cd C:\ThreatHuntingLab
```

A Python virtual environment was then created:
```command prompt:
python -m venv sigma-env
```

The virtual environment was activated using:
```command prompt:
sigma-env\Scripts\activate
```

After activation, the command prompt displayed the virtual environment:
```text
(sigma-env) C:\ThreatHuntingLab>
```

Using a virtual environment keeps the Sigma dependencies isolated from the system-wide Python installation.

## Installing Sigma CLI:
Sigma CLI was installed inside the virtual environment using pip:

```command prompt:
pip install sigma-cli
```

The installation was verified with:
```command prompt:
sigma --help
```

**The command successfully displayed the Sigma CLI help interface, confirming that Sigma CLI was installed, accessible from the VM.**

## Why Sigma Is Used?
Splunk SPL is specific to Splunk.

For example, a detection developed directly in Splunk may look conceptually like:
```text
Sysmon Event ID 1
+
PowerShell process
+
Suspicious command-line characteristics
```

Sigma allows the same detection idea to be represented using a standardized YAML-based rule format.
The detection engineering workflow used in this project is:

```text
Security Telemetry
        ↓
Threat Hunting in Splunk
        ↓
Suspicious Behavior Identified
        ↓
Develop SPL Detection
        ↓
Test and Tune Detection
        ↓
Create Sigma Rule
        ↓
Validate Detection Logic
        ↓
Document in GitHub
```

---

## Detection Repository Structure
Splunk and Sigma detections are maintained separately:

```text
detections/
├── splunk/
│   ├── suspicious-powershell.spl
│   └── ...
│
└── sigma/
    ├── suspicious-powershell.yml
    └── ...
```

This structure allows the project to demonstrate both:
- SIEM-specific detection engineering using SPL
- Portable detection logic using Sigma

---

## Role of Sigma in Threat Hunting:
Sigma itself is not a threat-hunting platform or SIEM.
Threat hunting in this lab is primarily performed using telemetry collected by:

```text
Windows Endpoint
       │
       ├── Sysmon
       │
       └── Windows Security Logs
       │
       ▼
Splunk Enterprise
       │
       ├── Threat Hunting
       ├── Investigation
       └── SPL Detection
```

Sigma is introduced after useful detection logic has been identified so that the detection can also be represented as Detection-as-Code.

## Verification:
The Sigma CLI installation was validated using:

```cmd
sigma --help
```

The command executed successfully without installation errors.

### Current Status

Component and Status:
Python Environment : ✅ Configured.
Python Virtual Environment: ✅ Configured.
Sigma CLI: ✅ Installed.
Sigma CLI Verification: ✅ Successful.
Splunk Backend: ⏳ Pending Validation.
Sigma → SPL Conversion: ⏳ Pending Validation.
First Custom Sigma Rule: ⏳ Pending.

---

## Next Step
The next stage of the lab is to configure and validate the **Sigma Splunk backend**.
After successful backend validation, a Sigma detection rule will be converted into Splunk-compatible detection logic and tested against telemetry collected in the lab.

This will establish the workflow:
```text
Threat Hunt
    ↓
Detection Development
    ↓
SPL
    ↓
Sigma Rule
    ↓
Sigma Validation
    ↓
Splunk-Compatible Detection
```

---

## Key Learning:
This setup introduced the concept of **Detection-as-Code**.
Instead of keeping detections only as queries inside a SIEM, detection logic can be stored as version-controlled files in GitHub, making it easier to review, maintain, test, and document security detections as the lab grows.
