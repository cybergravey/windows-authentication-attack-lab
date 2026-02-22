# 🛡 Blue Team Translation — Windows Authentication Attack Simulation

## 📌 Purpose

This document translates the offensive activity performed in the Windows Authentication Attack Simulation Lab into defensive detection opportunities.

The goal is to bridge attack behavior with monitoring, logging, and mitigation strategies that security teams can implement in real-world environments.

---

# 🔍 Attack Behavior Overview

The simulated attack chain included:

1. Network service discovery
2. SMB enumeration
3. Password spraying
4. Successful credential validation
5. RDP interactive logon

Each stage presents detection and prevention opportunities.

---

# 🚨 Detection Opportunities by Phase

---

## Phase 1 — Network Service Discovery

**Offensive Behavior**
- Nmap scan identifying exposed SMB (445) and RDP (3389)

**MITRE Mapping**
- T1046 — Network Service Discovery

### 🔎 Detection Signals

- Internal host performing sequential port scans
- High volume of connection attempts to multiple ports
- Firewall logs showing scanning behavior
- IDS/IPS alerts (e.g., Nmap fingerprinting signatures)

### 🛡 Defensive Controls

- Internal network segmentation
- IDS/IPS monitoring (Snort / Suricata rules)
- Alert on abnormal east-west scanning activity

---

## Phase 2 — SMB Enumeration

**Offensive Behavior**
- SMB service interrogation
- Identification of signing configuration
- Authentication requirement discovery

**MITRE Mapping**
- T1018 — Remote System Discovery

### 🔎 Detection Signals

- SMB session attempts from non-standard hosts
- Anonymous authentication attempts
- Event ID 4625 spikes (failed authentication)

### 🛡 Defensive Controls

- Enforce SMB signing
- Disable SMBv1
- Restrict SMB access via firewall rules
- Monitor unusual SMB authentication patterns

---

## Phase 3 — Password Spraying

**Offensive Behavior**
- Controlled password spray against SMB login

**MITRE Mapping**
- T1110.003 — Password Spraying
- T1078 — Valid Accounts (upon success)

### 🔎 Detection Signals

- Multiple failed login attempts across different usernames
- Event ID 4625 (Failed Logon)
- Logon Type 3 (Network Logon)
- Consistent password attempt across accounts
- Account lockout patterns

### 🛡 Defensive Controls

- Account lockout policies
- MFA enforcement
- Strong password policies
- SIEM alert for:
  - Failed-to-successful authentication sequence
  - Same source IP across multiple accounts

---

## Phase 4 — RDP Interactive Logon

**Offensive Behavior**
- Successful RDP login using valid credentials

**MITRE Mapping**
- T1021.001 — Remote Services (RDP)

### 🔎 Detection Signals

- Event ID 4624 (Successful Logon)
- Logon Type 10 (RemoteInteractive)
- New or unusual RDP source IP
- Concurrent session termination of active user
- Event ID 4634 (Logoff)

### 🛡 Defensive Controls

- Restrict RDP via firewall
- Enforce MFA for RDP
- Monitor abnormal RDP login patterns
- Alert on:
  - First-time RDP login from host
  - RDP login outside business hours
  - RDP login from non-admin workstation

---

# 📊 Correlated Detection Pattern

One of the strongest defensive indicators in this lab:

> Multiple failed authentication attempts followed by a successful login from the same source IP.

This pattern is a high-confidence signal of credential spraying.

Recommended SIEM Logic:

- Detect ≥ X failed logons from same source
- Followed by 1 successful logon
- Within Y-minute window
- On same target host

---

# 🧠 Strategic Observations

- Weak passwords remain a primary attack vector.
- Internal attack surface visibility is critical.
- Credential-based attacks often generate clear log signals when properly monitored.
- Service exposure combined with weak authentication increases risk significantly.

---

# 🎯 Defensive Takeaways

To reduce risk of similar attack paths:

- Enforce strong password policies
- Implement MFA for RDP
- Enforce SMB signing
- Monitor failed-to-successful logon patterns
- Restrict lateral movement paths
- Segment internal networks

---

# 📚 Why This Matters

Understanding the offensive workflow allows defenders to:

- Build stronger detection rules
- Improve incident response readiness
- Harden authentication controls
- Reduce lateral movement opportunities

This lab demonstrates how structured offensive simulation can directly inform defensive strategy.

---

**All activity documented here was conducted in a private, segmented homelab environment for educational and defensive research purposes only.**
