Project Overview

In this project, I worked as a SOC Analyst in a simulated enterprise environment at TECH THM. The main objective was to investigate security alerts inside an Endpoint Detection and Response (EDR) platform, analyze suspicious activity, and determine whether the detected behavior was malicious or legitimate.

During the investigation, I reviewed process trees, telemetry logs, file paths, and network connections to identify possible threats and understand attacker behavior.

The lab environment used an interactive EDR dashboard provided by TryHackMe.

Objectives
Investigate EDR detections
Analyze suspicious process activity
Identify malicious file execution
Review network telemetry and possible exfiltration attempts
Validate alerts using Threat Intelligence
Improve practical SOC investigation skills
Tools & Technologies
Endpoint Detection and Response (EDR)
Windows Process Analysis
Process Tree Investigation
Network Telemetry Analysis
Threat Intelligence
Windows Endpoint Security Monitoring
Investigation 1 — Malicious Payload Download
Host Investigated

DESKTOP-HR01

Alert Summary

The EDR platform detected suspicious activity originating from a malicious macro-enabled Microsoft Word document named invoice.docm.

The investigation revealed the following execution chain:

WINWORD.EXE → CMD.EXE → cURL.EXE → install.exe

The Microsoft Word document executed CMD.EXE, which then launched cURL.EXE to download a suspicious payload from an external source.

Findings
Tool Used to Download the Payload
cURL.exe
Downloaded Malware Location
C:\Users\Public\install.exe
Security Analysis

This behavior is highly suspicious because Office macro malware commonly abuses command-line utilities such as cmd.exe and curl.exe to download additional payloads.

The Public directory is also frequently used by attackers because it is writable by standard users and may avoid suspicion.

Investigation 2 — Credential Dumping Activity
Host Investigated

WIN-ENG-LAPTOP03

Alert Summary

The EDR detected an unsigned executable named syncsvc.exe running from the user's temporary directory.

The process attempted to access lsass.exe, which is commonly targeted by credential dumping tools to steal Windows credentials and authentication tokens.

Findings
Suspicious Executable Path
C:\Users\haris.khan\AppData\Local\Temp\syncsvc.exe
Suspicious Behavior Observed
Accessed lsass.exe
Attempted memory dump activity
Attempted outbound network communication
Executed from Temp directory
Unsigned binary
Security Analysis

This activity strongly indicates credential dumping and possible data exfiltration.

Attackers frequently target lsass.exe because it stores sensitive authentication data in memory. Running executables from the Temp directory is another major indicator of compromise.

The outbound upload attempt suggested that the attacker tried to exfiltrate stolen credential data to an external destination.

Investigation 3 — Threat Intelligence Validation
Host Investigated

DESKTOP-DEV01

Alert Summary

The EDR platform detected an executable named UpdateAgent.exe located in the roaming profile directory.

Although the binary was unsigned and generated suspicious outbound communication, Threat Intelligence analysis identified it as a legitimate internal IT utility.

Findings
File Location
C:\Users\daniel.richards\AppData\Roaming\UpdateAgent.exe
Threat Intelligence Label
Known internal IT utility tool
Security Analysis

This investigation demonstrates that not every EDR alert represents malicious activity.

SOC analysts must validate detections carefully before escalating incidents. Threat Intelligence and contextual analysis are critical to reducing false positives and avoiding unnecessary incident response actions.

Key Skills Demonstrated
EDR alert investigation
Process tree analysis
Malware detection
Credential dumping identification
Threat Intelligence validation
Security event triage
Windows endpoint analysis
What I Learned

This project helped me better understand how SOC analysts investigate endpoint alerts in real-world environments.

Key lessons learned:

Parent-child process relationships are critical during investigations
Temporary and public directories are common malware locations
Network telemetry helps identify suspicious activity
Threat Intelligence provides important context during alert triage
Not all alerts are malicious; validation is essential
Conclusion

Through this project, I gained hands-on experience analyzing endpoint security alerts using an EDR platform. I investigated malicious document execution, credential dumping behavior, and suspicious outbound activity while also learning how to identify legitimate internal tools.

This practical exercise improved my understanding of endpoint monitoring, incident investigation, and SOC analyst workflows in enterprise environments.
