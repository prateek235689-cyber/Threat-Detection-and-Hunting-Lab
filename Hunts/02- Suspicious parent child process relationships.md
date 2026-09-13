# Hunt 02 — Suspicious Parent-Child Process Relationships

## 🎯 Hunting Hypothesis: An attacker may abuse legitimate applications or processes to launch command shells,
scripting engines, or other processes in unusual parent-child relationships. Potentially suspicious activity may be identified by analyzing process ancestry and
comparing unusual relationships against normal endpoint behavior.

## 🔎 Data Source:
Data Source and Details
1. Endpoint Telemetry: Sysmon.
2. Event: Process Creation.
3. Sysmon Event ID: 1.
4. SIEM: Splunk Enterprise.
5. Primary Fields: Image, ParentImage, CommandLine, User.

## 🧠 Hunting Approach:
1. Establish a baseline of parent-child process relationships.
2. Identify commonly observed process relationships.
3. Search for unusual or low-frequency relationships.
4. Investigate process image, parent image, user, and command line.
5. Generate a safe controlled test.
6. Compare the controlled behavior against the baseline.
7. Develop behavioral SPL detection logic.
8. Evaluate potential false positives.
9. Represent the validated detection as a Sigma rule.
10. Validate the Sigma-derived detection in Splunk.

## 📊 Initial Telemetry Exploration:
The first stage analyzes existing Sysmon Process Creation events to understand normal
parent-child process relationships before developing detection logic.
