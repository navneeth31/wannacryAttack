# WannaCry Ransomware — VirtualBox Lab (Forensic / Defensive Report)

> *Important:* This repository documents a controlled lab exercise run in an isolated VirtualBox environment *for educational, defensive, and forensic analysis only. It does **not* include, nor will it provide, exploit code, working malware samples, operational playbooks, or step-by-step instructions that would enable replication or distribution of malicious tools. Do not attempt to run, reproduce, or distribute malware outside of authorized, legally compliant lab environments.

---

## Table of contents
- [Purpose](#purpose)
- [Disclaimer & Legal / Ethical Notice](#disclaimer--legal--ethical-notice)
- [Lab Environment (high-level)](#lab-environment-high-level)
- [High-level Incident Summary](#high-level-incident-summary)
- [Evidence & Artifacts collected (types)](#evidence--artifacts-collected-types)
- [Forensic Analysis (observations — non-actionable)](#forensic-analysis-observations---non-actionable)
- [Indicators of Compromise (IoC) — guidance](#indicators-of-compromise-ioc---guidance)
- [Detection & Hunting (what defenders should look for)](#detection--hunting-what-defenders-should-look-for)
- [Mitigation & Remediation](#mitigation--remediation)
- [Forensic & Incident Response Steps (recommended, high-level)](#forensic--incident-response-steps-recommended-high-level)
- [Lessons Learned](#lessons-learned)
- [Responsible Disclosure & Sharing](#responsible-disclosure--sharing)
- [References & Further Reading](#references--further-reading)

---

## Purpose
This repository documents a controlled, isolated lab exercise performed inside VirtualBox to study and analyze the behavior of ransomware in a safe environment. The goals are:
- Understand high-level ransomware behavior and propagation patterns.
- Collect and preserve forensic artifacts for analysis and training.
- Develop and test detection, mitigation, and remediation guidance for defenders.

---

## Disclaimer & Legal / Ethical Notice
- This work is for defensive research and education only.
- Running malware outside of an isolated, authorized lab is illegal and unethical.
- If you are documenting real incidents affecting production systems, follow your organization’s incident response and legal processes and do not publish sensitive artifacts.
- Do not rely on this document to reproduce, deploy, or weaponize malware.

---

## Lab Environment (high-level)
> Note: Do not include network-accessible malware, sample binaries, or raw exploit details in the repo.

Typical controlled lab baseline used (examples only — *not* instructions):
- Virtualization: VirtualBox (isolated host-only / internal network)
- Guest OS: Windows (patched status detailed below)
- Additional hosts: Linux jump host as monitoring/analysis point
- Tools used for observation: packet capture (PCAP), host process listing, memory acquisition tools, and offline forensic imaging

You should keep your lab completely offline or on a segmented internal network with no internet access when running analyses.

---

## High-level Incident Summary
- *Type:* Ransomware infection observed in an isolated VirtualBox lab.
- *Observed impact:* File encryption (affected files noted by unique extensions and ransom note files appearing), system/service disruption on infected VM(s), and attempted local & network propagation behavior.
- *Scope:* Confined to isolated lab network and virtual machine snapshots only.

> This summary purposefully avoids operational details and exact reproduction steps.

---

## Evidence & Artifacts collected (types)
The following artifact types were collected during the lab for defensive analysis and reporting. Do *not* include raw malware binaries in this repository.

- VM snapshots (preserve in secure storage; do not publish)
- Disk images and forensic images (retained offline)
- Memory dumps (volatile memory captures)
- Packet captures (PCAPs) from virtual network interfaces
- Host logs: Windows Event Logs, Sysmon logs, application logs
- Ransom note screenshots (redacted if containing identifiable info)
- Timeline exports (system timeline of relevant events)
- Process and service lists taken during and immediately after the incident
- Filenames and file extension examples (redacted/sanitized)

---

## Forensic Analysis (observations — non-actionable)
Below are high-level behavioral observations useful to defenders and analysts:

- *File activity:* Rapid modification/overwrite of user files; appearance of ransom note files in affected directories.
- *Process behavior:* Creation of processes that perform file I/O at scale, often with elevated privileges.
- *Persistence & startup:* Attempts to establish persistence via system startup/registry changes (general concept — not specific keys).
- *Network behavior:* Outbound network connections to hosts during or after the compromise; SMB-related scanning or connection attempts in cases of worm-like propagation.
- *Encryption patterns:* Files replaced/renamed and encrypted in place; original files often no longer readable by standard apps.
- *Ransom note presence:* Text files dropped in directories explaining payment and recovery steps.

> These points are intentionally general — they describe patterns rather than actionable reproduction steps.

---

## Indicators of Compromise (IoC) — guidance
- Share IoCs responsibly and only after ensuring they do not facilitate re-use of the malware.
- Preferred IoC formats: sanitized hashes (if safe), network connection patterns, filenames (redacted), and behavioral signatures used in EDR/SIEM rules.
- Before publishing IoCs, redact or sanitize anything that could serve as a working sample or command.

---

## Detection & Hunting (defensive guidance)
Use the following high-level signals to create detections — these are defensive patterns, not exploit instructions:

- Unusual high-volume file WRITE activity, especially across user document locations.
- Creation of ransom note files (monitor for specific patterns / filenames and content signatures).
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
- *Inventory & hardening:* Keep an accurate asset inventory and harden exposed services.

---

## Forensic & Incident Response Steps (recommended, high-level)
When responding to a suspected ransomware incident:
1. *Isolate* affected systems from networks (avoid shutting off systems prematurely if live forensic collection is needed).
2. *Preserve evidence:* Make forensic snapshots of system disks and memory where possible.
3. *Capture network traffic* and logs for relevant time window.
4. *Triage*: determine scope and identify affected hosts.
5. *Remediate*: restore from known good backups after ensuring root cause is fixed and systems are clean.
6. *Post-incident:* perform root cause analysis, lessons learned, and update playbooks.

> Follow your organization’s formal incident response process and legal requirements. Contact legal and law enforcement as appropriate.

---

## Lessons Learned
- Patch promptly and prioritize high-severity vendor bulletins.
- Ransomware can be mitigated strongly by good backups, segmentation, and endpoint defenses.
- Preparedness (playbooks, logs retention, and practiced restoration) shortens recovery time and reduces impact.
- Controlled labs are valuable for studying behavior — but must remain isolated and governed by clear rules.

---

## Responsible Disclosure & Sharing
- If you have artifacts that may help others, consider sharing sanitized summaries or YARA/Sigma rules (ensuring they do not contain malware or enable reproduction).
- Coordinate sharing with trusted information-sharing organizations (ISACs, vendor CERTs) where appropriate.
- Never upload raw malware binaries or live VM images into public repositories.

---

## References & Further Reading
(These are safe, non-actionable links to vendor advisories, incident response guidance, and defensive resources. Replace with the latest authoritative sources.)
- Microsoft security advisories and patch guidance (search MS17-010 and vendor guidance)
- CERT / national CSIRT ransomware advisories
- SANS / MITRE on ransomware detection and response
- Vendor whitepapers on detection and EDR tuning

---

## Contact / Attribution
- Author: Your Name  
- Lab date: YYYY-MM-DD (replace with the date of your exercise)
- Notes: All artifacts are stored securely and sanitized before any sharing.

---

### If you want:
- I can generate: a) a printable incident summary report based on this README, b) an executive one-page slide, or c) detection examples in Sigma format (sanitized, non-actionable). Tell me which and I’ll produce a safe version.
