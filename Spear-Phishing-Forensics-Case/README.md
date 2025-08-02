# Forensic Investigation of a Spear-Phishing & Data Exfiltration Incident


## Case Summary
- **Objective:** To conduct a full digital forensic investigation to determine the root cause of a data breach at M57dotBIZ. My primary goal was to resolve the conflicting statements between two senior executives and establish a factual timeline of how a sensitive employee spreadsheet was exfiltrated.
- **Scope:** The investigation centered on a single piece of evidence: a forensically sound `.E01` disk image of the CFO's computer (`nps-2008-jean.E01`).
- **Tools Used:** FTK Imager, Email Forensics Tools (PST Viewer).
- **Outcome:** My analysis conclusively proved that the CFO, Jean Jones, was the victim of a targeted spear-phishing attack. An external attacker, using the email `tuckgorge@gmail.com`, successfully spoofed the company President's identity to trick Ms. Jones into emailing them the confidential spreadsheet. The breach was a result of social engineering, not malicious insider action.

## Tools & Environment
| Tool | Purpose |
| :--- | :--- |
| **FTK Imager** | Mounting the disk image, verifying evidence integrity via hash checking, and exploring the file system. |
| **Email Forensics Tool (PST Viewer)** | Extracting and analyzing the `outlook.pst` file to reconstruct email conversations and examine headers. |

## Case Background
I was brought in as the lead forensic investigator for a high-stakes incident at M57dotBIZ. A confidential file containing employee PII and salaries was leaked, and the source was traced to the CFO's computer. The situation was tense, as the CFO, Jean Jones, claimed she sent the file at the request of the President, Alison Smith, who adamantly denied it. My task was to use digital evidence to cut through the conflicting stories, establish a definitive chain of events, and find the truth.


## Methodology
My investigation followed a strict, forensically sound methodology to ensure the integrity and admissibility of my findings.

1.  **Evidence Integrity Verification:** My first step was to verify the cryptographic hashes (MD5, SHA1) of the provided disk image (`nps-2008-jean.E01`) using FTK Imager. The hashes matched, confirming the evidence was an unaltered copy.
2.  **Evidence Mounting and Triage:** I mounted the verified image in FTK Imager to perform an initial survey of the file system. Based on the case background, I immediately identified the user's Outlook data file (`outlook.pst`) as the most critical artifact.
3.  **Artifact Extraction:** I forensically extracted the `outlook.pst` file from the disk image to a clean analysis environment. This isolated the key evidence for focused examination.
4.  **Email and Timeline Analysis:** I loaded the extracted PST file into an email analysis tool. I systematically reviewed the Inbox and Sent Items folders, reconstructing the email timeline around the date of the incident and paying close attention to email headers.
5.  **Evidence Correlation and Reporting:** I correlated the findings from the email analysis with file system metadata (e.g., the creation time of the spreadsheet) to build a complete, evidence-backed narrative of the incident.


## Findings & Evidence
The analysis of the `outlook.pst` file revealed a textbook spear-phishing attack. The attacker's actions and the victim's responses were clearly captured in the email artifacts.

**Indicator of Compromise (IoC):** `tuckgorge@gmail.com` (Attacker's Email)

| Artifact Type | Location / Value | Finding |
| :--- | :--- | :--- |
| **Spear-Phishing Email (The Lure)** | Jean Jones's Inbox (Received: 2008-07-20 02:22 AM) | An email impersonating Alison Smith created a false sense of urgency and authority to request the sensitive data. |
| **Spoofed Email Header (The Smoking Gun)** | `Return-Path: <simsong@xy.dreamhostps.com>` <br> `Envelope-From: tuckgorge@gmail.com` | Header analysis **irrefutably proved** the email did not originate from Alison Smith's account but from a Gmail address routed through a DreamHost server. |
| **Exfiltration Email (The Payload Delivery)** | Jean Jones's Sent Items (Sent: 2008-07-20 02:28 AM) | Believing the request was real, Jean Jones replied to the spoofed email, attaching `m57biz.xls`. The recipient field confirms the reply went to `tuckgorge@gmail.com`. |
| **Stolen File** | `m57biz.xls` (on Desktop) | The file containing employee SSNs and salaries was created just before the exfiltration, confirming it was made in direct response to the attacker's request. |


---

---

---

## Screenshots & Logs
The following visual evidence, extracted directly from the forensic image, provides irrefutable proof of the spear-phishing attack and data exfiltration.

**Exhibit A: Evidence Integrity Verification**
My first action was to verify the integrity of the disk image using FTK Imager. The matching MD5 and SHA1 hashes confirm that the evidence is an unaltered, forensically sound copy of the original hard drive, making all subsequent findings legally and technically defensible.

![Exhibit A: Hash verification in FTK Imager](https://raw.githubusercontent.com/Dorakhris/Spear-Phishing-Forensics-Case/master/Spear-Phishing-Forensics-Case/Images/exhibit-a-hash-verification.png)

**Exhibit B: The Spear-Phishing Email and Spoofed Header**
Analysis of the `outlook.pst` file revealed the attacker's lure. Note the deceptive `From:` field, designed to look like it came from "alison@m57.biz". The email header, however, provides the "smoking gun," proving the message originated from a DreamHost server (`dreamhostps.com`) and that the actual sending address was `tuckgorge@gmail.com`.

![Exhibit B: Screenshot of the spoofed email header](https://raw.githubusercontent.com/Dorakhris/Spear-Phishing-Forensics-Case/master/Spear-Phishing-Forensics-Case/Images/exhibit-b-spoofed-email.png)

**Exhibit C: The Exfiltration Event**
The "Sent Items" folder provides a clear record of the data exfiltration. This screenshot shows Jean Jones's reply, with the sensitive spreadsheet `m57biz.xls` attached, being sent directly to the attacker's `tuckgorge@gmail.com` address. This is the exact moment the data breach occurred.

![Exhibit C: Screenshot of the sent item with the malicious attachment](https://raw.githubusercontent.com/Dorakhris/Spear-Phishing-Forensics-Case/master/Spear-Phishing-Forensics-Case/Images/exhibit-c-exfiltration-sent-item.png)

**Exhibit D: Corroborating File System Evidence**
To complete the timeline, I analyzed the file system in FTK Imager. The metadata for `m57biz.xls` shows it was created on Jean's desktop at 1:28:03 AM on July 20, 2008. This is just one hour before she sent the exfiltration email, confirming she created the file specifically in response to the attacker's fraudulent request.

![Exhibit D: Screenshot of FTK Imager showing file metadata](https://raw.githubusercontent.com/Dorakhris/Spear-Phishing-Forensics-Case/master/Spear-Phishing-Forensics-Case/Images/exhibit-d-filesystem-metadata.png)

---


## Conclusion
My investigation conclusively determined that Jean Jones was not a malicious insider but a victim of a targeted social engineering attack. The data breach occurred because an external attacker successfully impersonated a high-level executive to manipulate an employee into violating policy and sending sensitive data. The root cause was not a failure of technology but a failure of human-layer security controls and awareness.

**Impact:** The incident resulted in a severe data breach of employee PII, created significant internal conflict, and exposed the company to legal and reputational risk.

**Recommendations:** My final report to M57dotBIZ leadership strongly recommended:
1.  **Technical Controls:** Implement email authentication standards (SPF, DKIM, DMARC) to make spoofing the company domain drastically harder. Configure the email gateway to flag emails from external sources that use internal employee display names.
2.  **Procedural Controls:** Mandate out-of-band verification (e.g., a phone call) for any request involving sensitive data or financial transfers, regardless of who appears to be asking.
3.  **Human Controls:** Immediately enroll all employees, especially executives, in mandatory and recurring security awareness training, using this incident as a real-world case study.


## Lessons Learned / Reflection
This case was a powerful reminder that the most sophisticated security stacks can be bypassed by exploiting the most vulnerable element: human trust. The entire incident hinged on the attacker's correct assumption that a request marked with urgency and authority would short-circuit an employee's critical thinking. The single most important lesson is that **out-of-band verification is one of the most effective, low-tech security controls an organization can implement.** A simple, 30-second phone call from Jean to Alison would have completely thwarted this attack. This investigation reinforced that a forensic analyst's job is not just to find data, but to reconstruct the human story behind it.



#DigitalForensics #DFIR #IncidentResponse #SpearPhishing #EmailForensics #SocialEngineering #FTKImager









