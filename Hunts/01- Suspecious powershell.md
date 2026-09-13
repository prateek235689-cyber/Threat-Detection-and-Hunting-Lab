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

## Initial Baseline Findings:
The initial PowerShell hunting query used a broad process-name match and returned a large number of events.

Analysis showed that the query was also matching Splunk processes such as:
- `splunk-powershell.exe`
- Splunk Universal Forwarder PowerShell processes

These events represented noise for the objective of this hunt rather than the Windows PowerShell executable being investigated.
The query was therefore refined to specifically target process paths ending in: `powershell.exe`

After refinement, the dataset was reduced to **14 Windows PowerShell process creation events** within the selected seven-day period.

The remaining events included PowerShell executions under different user contexts and parent processes, demonstrating that PowerShell execution alone is not sufficient to classify activity as suspicious.

## Sigma Detection:
After validating the behavioral detection in Splunk, the detection logic was represented as a Sigma rule.
The Sigma rule detects Windows PowerShell process creation and evaluates multiple command-line characteristics:

- `-NoProfile`
- `-NonInteractive`
- `-WindowStyle Hidden`
- `-EncodedCommand`

The rule requires at least two selected characteristics to be present before the activity is surfaced for investigation.

### Sigma Validation:
The rule was validated using Sigma CLI.

Validation result:
- Rule errors: 0
- Condition errors: 0
- Validation issues: 0

### Splunk Backend Conversion:
The Sigma Splunk backend was installed and verified.
The `splunk_windows` processing pipeline was used to convert the Sigma rule into Splunk SPL-compatible detection logic.
The generated logic was then adapted to the lab's Sysmon XML ingestion format because the `Image`, `CommandLine`, `ParentImage`, and `User` fields are extracted from raw XML during search.

### Detection Validation:
The Sigma-derived detection was executed against the Sysmon telemetry stored in Splunk.
The detection successfully identified the controlled PowerShell test containing:
- `-NoProfile`
- `-NonInteractive`
- `-WindowStyle Hidden`

One matching event was returned.

The event showed:
- PowerShell as the executed process
- `cmd.exe` as the parent process
- The monitored lab user as the execution context
- The complete PowerShell command line

This confirmed that the Sigma detection logic successfully identified the behavior it was designed to surface.

## Key Findings:
The hunt demonstrated that PowerShell execution alone is not sufficient to classify activity as suspicious.
Baseline analysis also showed that legitimate software may use arguments such as `-ExecutionPolicy Bypass`.
A behavioral approach using multiple execution characteristics therefore provided a more useful hunting signal than relying on a single PowerShell argument.

The investigation also demonstrated an important difference between portable Sigma detection logic and environment-specific SPL. The Sigma backend assumes normalized or extracted fields, while this lab currently stores Sysmon events as XML and performs field extraction at search time.

## MITRE ATT&CK Mapping
- Technique: Command and Scripting Interpreter: PowerShell
- Technique ID: T1059.001
- Tactic: Execution

## Hunt Result: **Status: Completed**
The hunting hypothesis was tested through baseline analysis, controlled simulation, behavioral detection development, false-positive analysis, Sigma rule creation, Sigma validation, Splunk conversion, and validation against endpoint telemetry.
