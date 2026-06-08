# Analysis Notes — FUTURE_CS_02

> Phishing Detection & Awareness Report  
> Author: [Jayram23](https://github.com/Jayram23)  
> Date: June 5, 2026

---

## Workflow Summary

1. Collected phishing email sample via CanIPhish simulation platform
2. Extracted raw email headers from the sample
3. Analyzed headers using MXToolbox Email Header Analyzer
4. Identified phishing indicators from both visual content and header data
5. Classified email risk level
6. Documented findings in the awareness report

---

## Sample 01 — Google Notification Impersonation

### Basic Info

| Field | Value |
|-------|-------|
| Subject | *(not extracted — visual sample only)* |
| Display Name | Google Notifications |
| Sender Address | `google-support@webnotifications[.]net` |
| Recipient | `john.doe@doperycorp[.]com` |
| Platform | CanIPhish (simulated) |
| Analysis Date | June 5, 2026 |

---

### Header Analysis — MXToolbox

**Authentication Results**

| Record | Result | Notes |
|--------|--------|-------|
| DMARC | ✅ Pass | Policy compliant |
| SPF Alignment | ✅ Pass | Sending IP matches SPF record |
| SPF Authentication | ✅ Pass | |
| DKIM Alignment | ❌ Fail | Domain mismatch |
| DKIM Authentication | ❌ Fail | Signature invalid or missing |

> **Key observation:** DMARC passes because SPF alone is sufficient for DMARC alignment in some configurations. However, DKIM failure means the email body/headers integrity cannot be cryptographically verified — a major red flag for a message supposedly from Google.

**Relay Hops**

| Hop | From | By | Protocol | Time (UTC) | Blacklist |
|-----|------|----|----------|------------|-----------|
| 1 | `EC2AMAZ-PVEOR5V 10.10.0.10` | `verify.caniphish.com` | ESMTP | 6/5/2026 11:27:18 PM | ✅ |
| 2 | `verify.caniphish.com 13.237.47.221` | `MR1PEPF00000D59.mail.protection.outlook.com` | TLS 1.3 / AES_256_GCM_SHA384 | 11:27:21 PM | ✅ |
| 3 | `MR1PEPF00000D59.FRAP264.PROD.OUTLOOK.COM` | `MR1P264CA0177.outlook.office365.com` | TLS 1.3 / AES_256_GCM_SHA384 | 11:27:23 PM | ✅ |
| 4 | `MR1P264CA0177.FRAP264.PROD.OUTLOOK.COM` | `PAXPR10MB5325.EURPRD10.PROD.OUTLOOK.COM` | TLS 1.2 / ECDHE_RSA_AES_256_GCM_SHA384 | 11:27:23 PM | ✅ |
| 5 | `PAXPR10MB5325.EURPRD10.PROD.OUTLOOK.COM` | `DB4PR10MB7520.EURPRD10.PROD.OUTLOOK.COM` | HTTPS | 11:27:24 PM | ❌ |

> **Key observation:** Origin is an AWS EC2 instance (`13.237.47.221`) routing through CanIPhish infrastructure — not Google's mail servers. Any legitimate Google notification would originate from Google-owned IP ranges. Hop 5 is blacklisted.

**Total received delay:** 6 seconds across 5 hops.

---

### Phishing Indicators

| # | Indicator | Category | Severity |
|---|-----------|----------|----------|
| 1 | Display name "Google Notifications" but domain is `webnotifications[.]net` | Sender Spoofing | 🔴 High |
| 2 | DKIM authentication failure | Header Anomaly | 🔴 High |
| 3 | Mail origin: AWS EC2, not Google infrastructure | Infrastructure Mismatch | 🔴 High |
| 4 | Fear-based content: "Sign-in attempt was blocked" | Social Engineering | 🟠 Medium |
| 5 | Deceptive CTA button: "Check activity" | Credential Harvesting | 🔴 High |
| 6 | Hop 5 blacklisted | Reputation | 🟠 Medium |

---

### Risk Classification

| Field | Value |
|-------|-------|
| Overall Risk | 🔴 HIGH — Phishing |
| Confidence | High |
| Attack Type | Brand impersonation + credential harvesting |
| Spoofed Brand | Google |
| Target Profile | Corporate email users |

---

## Tools Notes

### CanIPhish
- Used to generate a realistic phishing simulation email
- Provides pre-built templates mimicking real brands (Google, Microsoft, etc.)
- Useful for controlled awareness training and header analysis practice
- URL: [https://caniphish.com](https://caniphish.com)

### MXToolbox Email Header Analyzer
- Paste raw headers → instant SPF/DKIM/DMARC validation
- Relay hop visualization with timestamps and blacklist checks
- URL: [https://mxtoolbox.com/EmailHeaders.aspx](https://mxtoolbox.com/EmailHeaders.aspx)

---

## Personal Observations

- DMARC compliance does not guarantee a legitimate email — SPF alone can satisfy DMARC while DKIM fails
- The combination of DKIM failure + non-Google origin IP is sufficient to classify this as phishing with high confidence
- Real-world attackers often rely on users trusting the display name without checking the actual sender domain
- TLS encryption on relay hops does not indicate legitimacy — it only means the connection between mail servers is encrypted, not that the sender is trusted
