# Lab Architecture

## Environment:
The threat hunting lab uses an existing Windows endpoint monitoring environment.

**Component and Role:** 
1. Windows for Monitored endpoint.
2. Kali Linux for Controlled simulation system.
3. Sysmon for Endpoint telemetry collection.
4. Windows Security Logs for Authentication telemetry.
5. Splunk Enterprise for Centralized log analysis and hunting.

## Data Flow
Windows Endpoint
      |
      +---- Sysmon Events
      |
      +---- Windows Security Events
      |
      v
Splunk Enterprise
      |
      +---- Threat Hunting
      +---- SPL Detections
      +---- Investigation

Additional detection-as-code capabilities will be introduced using Sigma.
