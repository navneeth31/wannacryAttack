# WannaCry Ransomware — Offensive Report (VirtualBox Lab)

> *Important:* This repository documents a controlled lab exercise run in an **Isolated VirtualBox Environment** *for educational purpose only*. It does *not* include, *nor* will it provide, exploit code, working malware samples. Do not attempt to run, reproduce, or distribute malware outside of authorized, legally compliant lab environments.

<img width="100%" height="200" alt="Image" src="https://github.com/user-attachments/assets/945d43ef-2d17-4d59-8718-4fd7bd7dd187" />

## Table of contents
- [Purpose](#purpose)
- [Disclaimer & Legal / Ethical Notice](#disclaimer--legal--ethical-notice)
- [Lab Environment](#lab-environment)
- [Incident Summary](#incident-summary)
- [Evidence & Artifacts collected](#evidence--artifacts-collected)
- [Forensic Analysis](#forensic-analysis)
- [Detection & Hunting](#detection--hunting)
- [Mitigation & Remediation](#mitigation--remediation)
- [Lessons Learned](#lessons-learned)

---

## Purpose
This repository documents lab exercise performed inside *VirtualBox* to study and analyze the behavior of ransomware. The goals are:
- Understand ransomware behavior.
- Collect and preserve artifacts for analysis and training.

---

## Disclaimer & Legal / Ethical Notice
- This work is for offensive research and education purpose only.
- Running malware outside of an isolated, authorized lab is illegal and unethical.
- Do not rely on this document to *reproduce, deploy, or weaponize malware*.

---

## Lab Environment
Typical controlled lab baseline used:
- Virtualization: Oracle VirtualBox(KaliLinux)
- Guest OS: Windows 7
- Additional hosts: Linux jump host as monitoring/analysis point.
- Tools used for observation: nmap, Metasploit Framework.

---

## Incident Summary
- *Type:* Ransomware infection observed in an isolated VirtualBox lab.
- *Observed impact:* File encryption (affected files noted by unique extensions and ransom note files appearing), system/service disruption on infected VM(s).
- *Scope:* Confined to isolated lab network and virtual machine snapshots only.
- View, Download, Exploit the device

> This summary purposefully avoids operational details and exact reproduction steps.
---

## Evidence & Artifacts collected
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
- Ransom note screenshots
  <img width="1920" height="946" alt="Image" src="https://github.com/user-attachments/assets/fd2ff14f-32a6-40d3-85ba-61fd7a383003" />
- We can Download the info and attack system
  <img width="1920" height="946" alt="Image" src="https://github.com/user-attachments/assets/fd2ff14f-32a6-40d3-85ba-61fd7a383003" />
- The pages when system attacked
  ![Image](https://github.com/user-attachments/assets/c0a232eb-02f0-4498-abef-f58d866397ce)
  ![Image](https://github.com/user-attachments/assets/b55204b1-84d3-451f-8457-0addca079e8e)

---

## Forensic Analysis
Below are high-level behavioral observations useful to attackers and analysts:

- *File activity:* Rapid modification/overwrite of user files; appearance of ransom note files in affected directories.
- *Process behavior:* Creation of processes that perform file I/O at scale.
- *Encryption patterns:* Files replaced/renamed and encrypted in place; original files often no longer readable by standard apps.
- *Ransom note presence:* Text files dropped in directories explaining payment and recovery steps.

> These points are intentionally general — they describe patterns rather than actionable reproduction steps.

---

## Detection & Hunting
Use the following signals to create detections, not exploit instructions:

- Unusual high-volume file WRITE activity, especially across user document locations.
- Creation of ransom note files.
- Abrupt increase in failed file open/read errors for many files.
- New or abnormal service/process creation that performs mass file I/O.
- Sudden spikes in CPU or disk I/O on endpoints outside maintenance windows.

Suggested places to look:
- Windows Event Logs (Application/System/Security)
- Endpoint detection logs (EDR process and file events)
- Network flow logs and PCAPs for lateral movement attempts

---

## Mitigation & Remediation
Defensive steps to reduce risk and remediate infected hosts:
- *Patch management:* Ensure critical security updates are applied (e.g., patches addressing vulnerabilities used by wormable ransomware).
- *Backups:* Maintain immutable, offline or air-gapped backups and regularly test restoration procedures.
- *Endpoint protection:* Deploy and tune EDR/AV to detect mass file-modification behaviors and suspicious persistence techniques.
- *User training:* Educate users about phishing, suspicious attachments, and reporting processes.

---

## Lessons Learned
- Patch promptly and prioritize high-severity vendor bulletins.
- Preparedness (playbooks, logs retention, and practiced restoration) shortens recovery time and reduces impact.
- Controlled labs are valuable for studying behavior.
