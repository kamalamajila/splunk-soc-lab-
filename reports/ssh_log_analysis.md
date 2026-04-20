🔐 SSH Log Analysis using Splunk

This project demonstrates hands-on analysis of SSH logs using Splunk to detect suspicious login activity, brute-force attempts, and abnormal connection patterns.

The logs were ingested in JSON format and analyzed using Splunk’s Search Processing Language (SPL), simulating real-world SOC (Security Operations Center) investigations.

🛠️ Log Source Details
Field	Value
Source	ssh_logs.json
Sourcetype	_json
Host	DESKTOP-RTB43TD
🔍 Analysis & Findings
🔹 1. Basic Log Exploration
📸 Screenshot

🔎 Query
source="ssh_logs.json" host="DESKTOP-RTB43TD" sourcetype="_json"
📖 Explanation

This query retrieves all SSH log events to understand the dataset structure and key fields such as:

Source IP (id.orig_h)
Destination IP (id.resp_h)
Authentication status (auth_success)
Event type
Connection details
🎯 SOC Insight

Understanding the log structure is the first step in any investigation. It helps identify relevant fields for threat detection.

🔹 2. Failed SSH Login Attempts
📸 Screenshot

🔎 Query
source="ssh_logs.json" host="DESKTOP-RTB43TD" sourcetype="_json"
auth_success=false
| stats count as total_failed_attempts
📖 Explanation

Filters logs where authentication failed and counts total failed login attempts.

🚨 SOC Use Case
Detect brute-force attacks
Identify repeated login failures
Monitor unauthorized access attempts
🧠 Finding

A total of 608 failed login attempts were observed, indicating potential brute-force activity.

🔹 3. Failed Login Attempts by Source IP
📸 Screenshot

🔎 Query
source="ssh_logs.json" host="DESKTOP-RTB43TD" sourcetype="_json"
auth_success=false
| stats count by id.orig_h
| sort -count
📖 Explanation

Groups failed login attempts by source IP to identify attackers generating the most traffic.

🚨 SOC Use Case
Identify attacking IP addresses
Detect top brute-force sources
Prioritize IPs for blocking
🧠 Finding

Multiple internal IPs generated high failed login attempts, indicating suspicious activity patterns.

🔹 4. Event Type Analysis
📸 Screenshot

🔎 Query
source="ssh_logs.json" host="DESKTOP-RTB43TD" sourcetype="_json"
auth_success=false
| stats count by event_type
📖 Explanation

Categorizes failed login events by event type.

🚨 SOC Use Case
Understand attack behavior
Differentiate between login failures and brute-force attempts
🧠 Finding

Two major patterns were observed:

Failed SSH Login
Multiple Failed Authentication Attempts

This strongly suggests automated attack activity.

🔹 5. Top Attacking IPs
📸 Screenshot

🔎 Query
source="ssh_logs.json" host="DESKTOP-RTB43TD" sourcetype="_json"
auth_success=false
| stats count by id.orig_h
| sort -count
| head 10
📖 Explanation

Identifies the top 10 IP addresses generating failed login attempts.

🚨 SOC Use Case
Detect top attackers
Support incident response
Feed data into blocklists/firewalls
🧠 Finding

Certain IPs showed significantly higher activity, indicating likely brute-force sources.

🔗 MITRE ATT&CK Mapping

This analysis aligns with the following MITRE ATT&CK techniques:

T1110 – Brute Force
T1078 – Valid Accounts
T1046 – Network Service Scanning
✅ Conclusion

This SSH log analysis demonstrates how Splunk can be used to:

Detect brute-force login attempts
Identify malicious IP activity
Analyze authentication patterns
Support real-world SOC investigations

The project reflects practical SOC analyst workflows including log analysis, anomaly detection, and threat identification.
