# 📑 Cloud Incident Scenario 1: Multi-Stage Ransomware Investigation

## 🚨 Incident Overview

A sophisticated multi-stage ransomware attack was detected within the Azure cloud environment. The threat actor initially gained access through a phishing campaign targeting a privileged user account. Following successful compromise, the attacker attempted to establish persistence, perform lateral movement, and expand access across cloud resources before ransomware deployment.

---

## 🔍 Threat Hunting & Investigation

To assess the scope of the compromise and identify malicious activity, a structured threat-hunting process was conducted using Microsoft Sentinel and Microsoft Defender XDR.

### Step 1: Incident Triage – Identifying High-Severity Alerts

```kusto
AlertInfo
| where Severity == "High"
| project Timestamp, AlertId, Title, Severity
| top 10 by Timestamp desc
```

**Purpose:**
This query was used to identify the most critical security alerts generated within the environment and establish the initial attack timeline.

**Findings:**

* Detected a high-severity phishing alert.
* Confirmed the phishing activity as the initial access vector.
* Identified the affected user account and associated security events for further investigation.

**Outcome:**
Successfully validated the phishing campaign as a true positive security incident and established the starting point of the attack chain.

---

### Step 2: Evidence Investigation – Identifying Malicious Infrastructure

```kusto
AlertEvidence
| where EntityType in ("Account", "Ip")
| project Timestamp, AlertId, EntityType, EvidenceRole
| top 10 by Timestamp desc
```

**Purpose:**
This query was executed to examine entities associated with the detected alerts and identify attacker-controlled infrastructure involved in the compromise.

**Findings:**

* Discovered a suspicious external IP address linked to the phishing incident.
* Correlated attacker activity across multiple alerts.
* Confirmed communication patterns consistent with command-and-control (C2) activity.

**Outcome:**
Successfully identified and attributed malicious activity to an external threat source, enabling rapid containment actions.

---

## 🛑 Remediation & Mitigation

Following confirmation of malicious activity, immediate containment and remediation measures were implemented to prevent further compromise.

### Network Security Controls

* Blocked all identified malicious IP addresses through Azure Firewall and network security controls.
* Reviewed inbound and outbound traffic patterns for indicators of compromise.

### Identity Protection

* Revoked all active user sessions associated with affected accounts.
* Enforced password resets for impacted users.
* Implemented Conditional Access policies with mandatory Multi-Factor Authentication (MFA).

### Endpoint and Workload Containment

* Isolated compromised cloud workloads from production environments.
* Conducted forensic analysis on affected systems.
* Validated that no additional persistence mechanisms remained active.

### Monitoring Enhancements

* Increased alerting sensitivity for phishing-related detections.
* Deployed additional Sentinel analytics rules for ransomware indicators.
* Enhanced continuous monitoring for suspicious authentication and lateral movement activities.

---

## ✅ Lessons Learned

This investigation demonstrated the importance of rapid alert triage, evidence correlation, and proactive containment measures. Early identification of the phishing campaign enabled security teams to disrupt the attack lifecycle before widespread ransomware encryption could occur, significantly reducing organizational risk and operational impact.
