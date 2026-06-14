# FUTURE_CS_02 — Phishing Detection & Awareness Report

**KPOGO K. Samuel** | Cybersecurity Student — L2 Licence Pro  
ESIG Global Success | Lomé, Togo  
🔗 GitHub: [github.com/Jayram23](https://github.com/Jayram23)  
🔗 LinkedIn: [linkedin.com/in/skpogo](https://linkedin.com/in/skpogo)

---

## Overview

Phishing remains one of the most prevalent and costly attack vectors in modern cybersecurity. This project simulates the work of a real security analyst: collecting phishing email samples, performing header analysis, identifying attack indicators, and producing a client-ready awareness report.

This is a **defensive security and education** project — no systems were attacked or compromised.

---

## Objectives

- Analyze real-world phishing email samples
- Perform email header forensics (SPF, DKIM, DMARC, relay hops)
- Identify and classify phishing indicators
- Produce a professional awareness report usable by organizations

---

## Tools Used

| Tool | Purpose |
|------|---------|
| [CanIPhish](https://caniphish.com) | Phishing email simulation & sample collection |
| [MXToolbox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx) | Header parsing, relay analysis, DMARC/SPF/DKIM validation |
| Microsoft Word | Report writing and documentation |

---

## Repository Structure

```
FUTURE_CS_02/
├── README.md                  # This file
├── report/
│   └── Phishing_Detection_Awareness_Report_2026.docx
├── evidence/
│   ├── 01_phishing_email_sample.png
│   ├── 02_header_analysis_mxtoolbox.png
│   ├── 03_header_relay_hops.png
│   ├── 04_header_relay_details.png
│   └── EVIDENCE_INDEX.md
└── tools/
    └── analysis_notes.md
```

---

## Sample Analysis Summary

### Email Analyzed
- **Subject:** Welcome to CanIPhish *(phishing simulation)*
- **Spoofed Sender:** `Google Notifications <google-support@webnotifications[.]net>`
- **Target:** `john.doe@doperycorp[.]com`

### Header Analysis Results

| Check | Result |
|-------|--------|
| DMARC | ✅ Compliant |
| SPF Alignment | ✅ Pass |
| SPF Authentication | ✅ Pass |
| DKIM Alignment | ❌ Fail |
| DKIM Authentication | ❌ Fail |
| Received Delay | 6 seconds |
| Relay Hops | 5 hops (AWS EC2 → Outlook) |

### Key Phishing Indicators Identified

1. **Sender domain mismatch** — Display name claims "Google Notifications" but domain is `webnotifications[.]net` (not `google.com`)
2. **DKIM failure** — Email signature cannot be verified, indicating the sender domain does not legitimately authorize this mail
3. **Fear-based content** — Message claims a sign-in attempt was blocked to trigger panic and urgent action
4. **Deceptive CTA** — "Check activity" button designed to harvest credentials
5. **Non-Google infrastructure** — Mail originates from an AWS EC2 instance (`EC2AMAZ-PVEOR5V 10.10.0.10`), not Google servers

---

## Risk Classification

| Category | Level |
|----------|-------|
| Overall Risk | 🔴 **HIGH — Phishing** |
| Spoofing Technique | Brand impersonation (Google) |
| Attack Goal | Credential harvesting |
| Target Profile | Corporate email users |

---

## Key Takeaways

- Always verify the **actual sender domain**, not just the display name
- DKIM failure on a legitimate service (Google, Microsoft) is a strong phishing signal
- Urgency + fear language + suspicious link = classic phishing pattern
- Organizations should enforce **DMARC reject policies** to block spoofed emails at the gateway

---

## Full Report

See [`report/Phishing_Detection_Awareness_Report_2026.docx`](report/Phishing_Detection_Awareness_Report_2026.docx) for the complete analysis including prevention guidelines and employee awareness recommendations.

---

## Disclaimer

All email samples used in this project are **simulated phishing emails** from CanIPhish, a legitimate security awareness platform. No real users or systems were targeted. This project is strictly for educational and portfolio purposes.
