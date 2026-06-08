# Evidence Index — FUTURE_CS_02

> Phishing Detection & Awareness Report  
> Author: [Jayram23](https://github.com/Jayram23)

---

## Evidence Files

| # | Filename | Tool | Description |
|---|----------|------|-------------|
| 01 | `01_phishing_email_sample.png` | CanIPhish | Simulated phishing email impersonating Google Notifications. Sender domain `google-support@webnotifications[.]net`, target `john.doe@doperycorp[.]com`. Displays fear-based content ("Sign-in attempt was blocked") with a deceptive "Check activity" CTA. |
| 02 | `02_header_analysis_mxtoolbox.png` | MXToolbox | MXToolbox header analysis overview. Shows DMARC Compliant, SPF Alignment ✅, SPF Authenticated ✅, DKIM Alignment ❌, DKIM Authenticated ❌. Received delay: 6 seconds. Relay timeline graph (AWS EC2 → Outlook infrastructure). |
| 03 | `03_header_relay_hops.png` | MXToolbox | Relay hop table (From / By columns). 4 hops visible: origin `EC2AMAZ-PVEOR5V 10.10.0.10` → `verify.caniphish.com` → Microsoft Outlook mail protection servers. Confirms mail originated from AWS EC2, not Google infrastructure. |
| 04 | `04_header_relay_details.png` | MXToolbox | Full relay details table (From / By / With / Time UTC / Blacklist columns). 5 hops total. All hops use TLS encryption (TLS 1.2 / TLS 1.3). Hop 5 flagged with blacklist indicator ❌. Timestamps all at 6/5/2026 ~11:27 PM UTC. |

---

## Collection Method

All evidence was collected on **June 5, 2026** using:
- **CanIPhish** — phishing simulation platform for generating realistic email samples
- **MXToolbox Email Header Analyzer** — for parsing raw email headers and validating authentication records (SPF, DKIM, DMARC)

No real users or live systems were targeted. All samples are simulated in a controlled environment.

---

## Notes

- Screenshots were captured directly from the analysis tools
- Sensitive domains and IP addresses are defanged where applicable (brackets around `.`)
- Evidence files are numbered in the order of the analysis workflow: sample → header overview → relay hops → relay details
