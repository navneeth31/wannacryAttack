# WannaCry Ransomware — Forensic / Offensive Report (VirtualBox Lab)

> *Important:* This repository documents a controlled lab exercise run in an isolated VirtualBox environment *for educational, offensive, and forensic analysis only*. It does *not* include, *nor* will it provide, exploit code, working malware samples, operational playbooks, or step-by-step instructions that would enable replication or distribution of malicious tools. Do not attempt to run, reproduce, or distribute malware outside of authorized, legally compliant lab environments.

<img width="100%" height="200" alt="Image" src="https://github.com/user-attachments/assets/945d43ef-2d17-4d59-8718-4fd7bd7dd187" />

## Table of contents
- [Purpose](#purpose)
- [Disclaimer & Legal / Ethical Notice](#disclaimer--legal--ethical-notice)
- [Lab Environment (high-level)](#lab-environment-high-level)
- [High-level Incident Summary](#high-level-incident-summary)
- [Evidence & Artifacts collected (types)](#evidence--artifacts-collected-types)
- [Forensic Analysis (observations — non-actionable)](#forensic-analysis-observations---non-actionable)
- [Detection & Hunting (what defenders should look for)](#detection--hunting-what-defenders-should-look-for)
- [Mitigation & Remediation](#mitigation--remediation)
- [Forensic & Incident Response Steps (recommended, high-level)](#forensic--incident-response-steps-recommended-high-level)
- [Lessons Learned](#lessons-learned)
- [References & Further Reading](#references--further-reading)

---

## Purpose
This repository documents a controlled, isolated lab exercise performed inside *VirtualBox* to study and analyze the behavior of ransomware in a safe environment. The goals are:
- Understand high-level ransomware behavior and propagation patterns.
- Collect and preserve forensic artifacts for analysis and training.

---

## Disclaimer & Legal / Ethical Notice
- This work is for defensive research and education only.
- Running malware outside of an isolated, authorized lab is illegal and unethical.
- If you are documenting real incidents affecting production systems, follow your organization’s incident response and do not publish sensitive artifacts.
- Do not rely on this document to *reproduce, deploy, or weaponize malware*.

---

## Lab Environment (high-level)

Typical controlled lab baseline used (examples only — *not* instructions):
- Virtualization: VirtualBox (isolated host-only / internal network)
- Guest OS: Windows (patched status detailed below)
- Additional hosts: Linux jump host as monitoring/analysis point
- Tools used for observation: nmap, Metasploit module,Meterpreter, Metasploit Framework.

You should keep your lab completely offline or on a segmented internal network with no internet access when running analyses.

---

## High-level Incident Summary
- *Type:* Ransomware infection observed in an isolated VirtualBox lab.
- *Observed impact:* File encryption (affected files noted by unique extensions and ransom note files appearing), system/service disruption on infected VM(s), and attempted local & network propagation behavior.
- *Scope:* Confined to isolated lab network and virtual machine snapshots only.
- View, Download, Exploit the device

> This summary purposefully avoids operational details and exact reproduction steps.
---

## Evidence & Artifacts collected (types)
The following artifact types were collected during the lab for offensivse analysis and reporting.

- VM snapshots (Starting the process)
  <img width="1060" height="598" alt="Image" src="https://github.com/user-attachments/assets/3b6bff63-5380-49de-8189-2132fc09e6bf" />
- The *Metasploit* Interface
  <img width="1060" height="598" alt="Image" src="https://github.com/user-attachments/assets/a2915ab7-5da6-4f91-9430-e494403a9cc6" />
- Searching for target device on the NMap findings
  <img width="1060" height="598" alt="Image" src="https://github.com/user-attachments/assets/3d40f778-afe5-42eb-be8a-43672c676ed9" />
- We will have a option for a *target* type
  <img width="1920" height="946" alt="Image" src="https://github.com/user-attachments/assets/872ddd89-dbf1-4710-a27a-23beacd3fece" />
- Run the attack anonymously on the *target* device
  <img width="1920" height="946" alt="Image" src="https://github.com/user-attachments/assets/a5603ce4-2047-4df6-a0d7-7e138f7436a9" />
- Ransom note screenshots (redacted if containing identifiable info)
  <img width="1920" height="946" alt="Image" src="https://github.com/user-attachments/assets/fd2ff14f-32a6-40d3-85ba-61fd7a383003" />
- We can Download the info and attack system
  <img width="1920" height="946" alt="Image" src="https://github.com/user-attachments/assets/fd2ff14f-32a6-40d3-85ba-61fd7a383003" />
- The pages when system attacked(tried to view any info this pages appear)
  ![Image](https://github.com/user-attachments/assets/c0a232eb-02f0-4498-abef-f58d866397ce)
  --
  ![Image](https://github.com/user-attachments/assets/b55204b1-84d3-451f-8457-0addca079e8e)

---

## Forensic Analysis (observations — non-actionable)
Below are high-level behavioral observations useful to attackers and analysts:

- *File activity:* Rapid modification/overwrite of user files; appearance of ransom note files in affected directories.
- *Process behavior:* Creation of processes that perform file I/O at scale, often with elevated privileges.
- *Network behavior:* Outbound network connections to hosts during or after the compromise.
- *Encryption patterns:* Files replaced/renamed and encrypted in place; original files often no longer readable by standard apps.
- *Ransom note presence:* Text files dropped in directories explaining payment and recovery steps.

> These points are intentionally general — they describe patterns rather than actionable reproduction steps.

---

## Detection & Hunting (defensive guidance)
Use the following high-level signals to create detections, not exploit instructions:

- Unusual high-volume file WRITE activity, especially across user document locations.
- Creation of ransom note files.
- Abrupt increase in failed file open/read errors for many files.
- New or abnormal service/process creation that performs mass file I/O.
- Unusual SMB traffic patterns (scanning, unexpected connections between endpoints).
- Sudden spikes in CPU or disk I/O on endpoints outside maintenance windows.

Suggested places to look:
- Windows Event Logs (Application/System/Security)
- Endpoint detection logs (EDR process and file events)
- Network flow logs and PCAPs for lateral movement attempts
- SIEM correlation for file-modification spikes and unusual process chains

---

## Mitigation & Remediation
Defensive steps to reduce risk and remediate infected hosts (high-level):
- *Patch management:* Ensure critical security updates are applied (e.g., patches addressing vulnerabilities used by wormable ransomware). Keep systems patched per vendor guidance.
- *Disable legacy protocols:* Disable SMBv1 and other deprecated protocols if not required.
- *Backups:* Maintain immutable, offline or air-gapped backups and regularly test restoration procedures.
- *Network segmentation:* Limit lateral movement by segmenting networks and applying least-privilege access.
- *Endpoint protection:* Deploy and tune EDR/AV to detect mass file-modification behaviors and suspicious persistence techniques.
- *User training:* Educate users about phishing, suspicious attachments, and reporting processes.
---

## Forensic & Incident Response Steps (recommended, high-level)
When responding to a suspected ransomware incident:
1. *Isolate* affected systems from networks (avoid shutting off systems prematurely if live forensic collection is needed).
2. *Preserve evidence:* Make forensic snapshots of system disks and memory where possible.
3. *Capture network traffic* and logs for relevant time window.
4. *Triage*: determine scope and identify affected hosts.
5. *Remediate*: restore from known good backups after ensuring root cause is fixed and systems are clean.
6. *Post-incident:* perform root cause analysis, lessons learned, and update playbooks.

---

## Lessons Learned
- Patch promptly and prioritize high-severity vendor bulletins.
- Preparedness (playbooks, logs retention, and practiced restoration) shortens recovery time and reduces impact.
- Controlled labs are valuable for studying behavior — but must remain isolated and governed by clear rules.

---

## References & Further Reading
(These are safe, non-actionable links to vendor advisories, incident response guidance, and defensive resources. Replace with the latest authoritative sources.)
- [Microsoft security advisories and patch guidance](https://learn.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010)
- [SANS / MITRE on ransomware detection and response](https://isc.sans.edu/)
- [Malware Traffic Analysis](https://www.malware-traffic-analysis.net/)
