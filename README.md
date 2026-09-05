# Digital Forensic Investigation NIST/CFReDS Hacking Case

**Case ID:** CIP-B104-CS1-C11_26_DFIT_17305
**Examiner:** NASIRU LAWAL
**Evidence Source:** [NIST CFReDS](https://cfreds-archive.nist.gov/) "Hacking Case" (SCHARDT.001–008)

## Overview

This repository contains the complete forensic examination of a disk image recovered from a Dell CPi notebook, associated with a suspect operating under the online alias **"Mr. Evil"** and the registered name **Greg Schardt**. The computer is alleged to have been used to identify wireless access points, intercept internet traffic, and capture credentials from nearby users.

The investigation follows a structured methodology covering evidence integrity, file-system and Registry analysis, suspect attribution, hacking-tool inventory, captured-traffic reconstruction, deleted-file recovery, and malware scanning concluding with an integrated timeline and a calibrated forensic opinion.

## Repository Structure

```
├── 01_Report/                  Final PDF forensic report
├── 02_Hashes/                  hash_manifest.csv — evidence & artefact hashes (MD5/SHA-256)
├── 03_Timeline/                timeline.csv — chronological UTC activity timeline
├── 04_Evidence_Register/       evidence_register.csv — full evidence tracking table
├── 05_Screenshots/             Numbered figures referenced from the report (Fig01–Fig19)
├── 06_Command_Outputs/         Saved raw command/tool output (.txt)
├── 07_Artefacts/               Only the specific recovered files needed to support findings
└── README.md
```

## Tools Used

| Purpose | Tool |
|---|---|
| Partition & file-system analysis | The Sleuth Kit (`mmls`, `fls`, `icat`) |
| Registry parsing | RegRipper (`rip.pl`) |
| Event log parsing | `evtparse.pl` |
| Recycle Bin analysis | `rifiuti2` |
| Packet capture analysis | `tshark` / Wireshark |
| Malware scanning | ClamAV (`clamscan`) |
| Platform | Kali Linux |

## Key Findings (Summary)

- System: Windows XP, registered to **Greg Schardt**, computer name `N-1A9ODN6ZXK4LQ`, workgroup **EVIL**
- Primary account **"Mr. Evil"** (RID 1003) — 15 logins, last active 2004-08-27
- Multiple independent artefacts (Look@LAN config, browser cache, webmail, IRC/NNTP identity) link **Greg Schardt ↔ Mr. Evil**
- Six+ dual-use network/security tools installed: Ethereal, WinPcap, Network Stumbler, Cain & Abel, Anonymizer Bar, 123 Write All Stored Passwords, CuteFTP
- A recovered packet capture (`interception`) contains genuine HTTP traffic from a Windows CE/Pocket PC device to Microsoft Passport login pages — direct evidence of interception in use, not just installed
- Several hacking-tool installers were deleted to the Recycle Bin within minutes of installation, suggesting deliberate cleanup
- ClamAV scan flagged 22 files; 21 are category hits on pen-testing tools, 1 (`ahui.exe`) is a genuine `Win.Virus.Virut` infection

Full findings, evidence, and the calibrated final conclusion are documented in [`01_Report/`](./01_Report).

## Reproducing This Analysis

```bash
mkdir hacking_case && cd hacking_case
for i in 001 002 003 004 005 006 007 008; do
  wget https://cfreds-archive.nist.gov/images/hacking-dd/SCHARDT.$i
done
cat SCHARDT.00* > SCHARDT.dd
md5sum SCHARDT.dd       
sha256sum SCHARDT.dd    
mmls SCHARDT.dd          
```

See `06_Command_Outputs/` for the full set of commands and their captured output used throughout this examination.

## Disclaimer

This is an academic digital forensics exercise using a publicly available NIST CFReDS reference dataset created for training and tool-testing purposes. No real individuals, victims, or live systems are involved. Any password or credential values recovered from the evidence have been redacted from this repository.

## Author

**NASIRU LAWAL**
Case ID: CIP-B104-CS1-C11_26_DFIT_17305
