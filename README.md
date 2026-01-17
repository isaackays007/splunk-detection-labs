# splunk-detection-labs

Splunk SIEM home lab focused on SSH and authentication attack detection, SPL correlation searches, and alerting. Includes lab architecture, linux_secure log onboarding, brute-force/wrong-password detections, and dashboards for SOC-style investigations.

## Overview

This repo documents a Splunk-based SIEM home lab used to detect and investigate SSH brute-force and repeated wrong-password activity. It covers lab architecture, data onboarding, SPL searches, scheduled alerts, and dashboards for SOC-style investigations.

## Lab Setup

- Hypervisor: VirtualBox (local home lab)
- Splunk Server: Ubuntu Server running Splunk Enterprise (free license)
- Attacker VM: Kali Linux / Linux Lite box generating SSH brute-force and wrong-password attempts
- Target VM: Ubuntu Server sending linux_secure and auth logs
- Log Ingestion: Splunk Universal Forwarder on the target VM forwarding linux_secure/auth logs to the Splunk indexer (TCP)
  
## Lab Architecture

- Splunk Enterprise running on a Linux VM
- Attacker VM generating SSH brute-force and wrong-password attempts
- Target VM sending linux_secure and auth logs to Splunk via forwarder or syslog
- Index and sourcetype configuration for linux_secure events

## Detection Goals

- Identify SSH brute-force attacks based on repeated failed logins by source IP
- Detect repeated wrong-password attempts against specific accounts
- Trigger scheduled alerts when thresholds are exceeded
- Visualize authentication activity for SOC-style investigations

## SPL Examples

```spl
index=main sourcetype=linux_secure "Failed password"
| stats count BY src_ip
| where count > 5
```                                                                                                                                       
## Planned Enhancements

- Add additional SPL detections (per-user thresholds, success-after-fail patterns, and geo/anomaly-based SSH activity)
- Ingest web application logs (e.g., DVWA/Juice Shop) for multi-source correlation
- Document example alert configurations, incident investigation steps, and screenshots of dashboards
