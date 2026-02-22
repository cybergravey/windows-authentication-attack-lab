# 🖥 Windows Authentication Attack Simulation Lab

## 📌 Overview

This repository documents a structured credential-based attack simulation conducted inside a segmented homelab environment.

The objective of this lab was to understand how weak authentication controls and exposed services can translate into real-world access and how those behaviors can be detected and mitigated from a defensive perspective.

This project emphasizes methodology, documentation, and MITRE ATT&CK mapping rather than exploitation.

---

## 🔧 Lab Architecture

**Environment Type:** Private homelab (internal subnet, no internet exposure)

**Attacker Machine:**
- Kali Linux

**Target Machine:**
- Windows 11 Pro workstation  
  *(Service fingerprinting identifies NT 10.0 — consistent with Windows 11 kernel versioning)*

**Exposed Services:**
- SMB (TCP 445)
- RDP (TCP 3389)

---

## 🧭 Attack Methodology

The simulation followed a structured lifecycle:

### 1. Reconnaissance
- Network service discovery using Nmap
- Identification of exposed SMB and RDP services
- Confirmation of target OS via service fingerprinting

### 2. Enumeration
- SMB inspection
- Service configuration analysis
- Identification of authentication requirements

### 3. Credential Testing
- Controlled password spray simulation against SMB
- Weak credential identified for local user account

### 4. Remote Access Validation
- RDP interactive logon using valid credentials
- Demonstration of full desktop-level access

### 5. Framework Validation
- Credential confirmation using Metasploit auxiliary modules
- Verification of credential reuse viability

---

## 🧠 MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|--------|-----------|----|
| Discovery | Network Service Discovery | T1046 |
| Discovery | Remote System Discovery | T1018 |
| Credential Access | Password Spraying | T1110.003 |
| Initial Access | Valid Accounts | T1078 |
| Lateral Movement | Remote Services (RDP) | T1021.001 |

A full MITRE ATT&CK mapping PDF is included in the `/docs` directory.

---

## 🛡 Blue Team Detection Considerations

This lab also includes defensive translation and detection opportunities:

- Event ID 4625 — Failed logon attempts
- Event ID 4624 — Successful logon
- Logon Type 10 — RemoteInteractive (RDP)
- Failed-to-successful authentication pattern
- Abnormal RDP source IP

A structured incident-style report is included in `/docs`.

---

## 🔎 Key Observations

- Weak password hygiene remains one of the most effective attack vectors.
- Exposed internal services increase attack surface.
- Credential reuse significantly accelerates compromise.
- OS fingerprinting relies on protocol responses — Windows 11 reports NT 10.0 for compatibility.

---

## 🎯 Objective

This lab was conducted strictly for educational and defensive research purposes inside a private environment.

The goal was to:

- Understand authentication attack paths
- Map activity to MITRE ATT&CK
- Translate offensive activity into defensive detection strategies
- Improve structured security documentation skills

See full mapping in `/docs/MITRE_ATTACK_Mapping.pdf`

See incident summary in `/docs/Windows_Incident_Summary.pdf`

---

## 📂 Repository Structure

```
windows-authentication-attack-lab/
│
├── docs/
│   ├── MITRE_ATTACK_Mapping.pdf
│   └── Windows_Incident_Summary.pdf
│
├── media/
│   └── lab_demo.mp4
│
├── playbook/
│   ├── windows_attack_playbook.md
│   └── blue_team_translation.md
│
└── README.md
```

---

## 📚 Continuous Learning

Understanding how attacks work at a technical level strengthens:

- Troubleshooting
- Risk assessment
- Incident response readiness
- Defensive engineering mindset

Always building. Always learning.
