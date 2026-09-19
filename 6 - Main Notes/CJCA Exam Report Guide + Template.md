
2026-09-18 19:30

Status: active

Tags: [[3 - Tags/reporting|reporting]],[[soc]],[[3 - Tags/incident-reponse|incident-reponse]],[[htb]]


# CJCA Exam Report — Guide + Template

> Section structure mirrors the **official `HTB-CJCA-Report` template**. The exam hands you the template as a **`.docx`** (rename to `.zip` and extract to get the alerts table). Fill the `TODO` fields, export to **PDF**, and submit. This note is your pre-loaded structure + writing craft so you're not learning the format under a 5-day clock.

---

## 0. The reality — what actually gets you the pass (confirmed from the Letter of Engagement)

- **Format:** 5-day (~120h), 2 phases. Target domain **`luminex.htb`** (client: Luminex Ltd; you're a Jr analyst at Acme Security).
- **Phase 1 – Gray box pentest = 100 pts, need ≥80 to pass.** 6 in-scope hosts:
  - `NIX01` (software dev server) · `NIX02` (email server) · `WEB01` (web server) · `WIN01` (Windows client) · `WIN02` (Windows mgmt server) · `luminex.htb` (domain)
  - `user.txt` + `root.txt` per host. Treat it as a **busy corporate network with live users**.
- **Phase 2 – SIEM alert validation: classify ≥27 alerts correctly.** Elastic SIEM on **`ELK` host, port 5601, creds `elastic/elastic`**. **ELK is OUT of scope for Phase 1 — never scan/attack it.** Alerts table lives inside the `.docx` template.
- **The report is a hard gate.** Full points + a weak report = FAIL. **Report submission is also required to even qualify for a 2nd attempt / get the cert.**
- **Submission:** password-protected zip → `zip -P hackthebox cjca_report.zip report.pdf alerts.csv` → upload.

**The four things that fail reports:** (1) a flag value with no reproducible steps behind it, (2) a flag screenshot **missing the `date` command output** (mandatory — see below), (3) an alert verdict with no log evidence, (4) screenshots that don't show command + result + target IP in-frame.

> **Integrity note:** solving the boxes and classifying the actual alerts is your work — this note is structure, formatting, evidence discipline, and submission mechanics only.

---

## 1. Official section map (fill each `TODO`)

```
1  Statement of Confidentiality        <- boilerplate, leave as-is
2  Engagement Contacts                 <- your candidate name / HTB as client
3  Executive Summary
   3.1 Approach
4  Assignment
   4.1 Objective
   4.2 Assignment Overview
5  Phase 1: Grey Box Penetration Test
   5.1 Scope
   5.2 Reporting and Deliverables
   5.3 Rules of Engagement
6  Network Penetration Test Assessment Summary
   6.1 Summary of Findings            <- severity table (the graded overview)
   6.2 Exploited Hosts
   6.3 Compromised Users
   6.4 Changes / Host Cleanup         <- artifacts you dropped (linpeas, shells...)
7  Internal Network Compromise Walkthrough
   7.1 Detailed Walkthrough           <- step table: Step# | Host | Action/Command | Result
   7.2 Collected Evidence             <- terminal output + screenshots per step
8  Remediation Summary
   8.1 Short Term  /  8.2 Medium Term  /  8.3 Long Term
9  Phase 2: SIEM Alert Validation and Analysis
   9.1 Scope
   9.2 Deliverables
   9.3 SIEM Alerts                     <- the given alert list (name/desc/host/timestamp)
   9.4 SIEM Alert Validation and Analysis   <- your verdict table (TP/FP + evidence)
A  Appendix
   A.1 Finding Severities              <- boilerplate rating definitions
   A.2 Flags Discovered               <- Host | user.txt/root.txt | flag value
```

---

## 2. Phase 1 — the graded tables (copy these)

### 6.1 Summary of Findings
> One row per real issue you exploited. Severity by impact, not by how cool it was.

| # | Severity | Finding Name |
|---|----------|--------------|
| 1 | High | Exposed and Unprotected SSH Private Key |
| 2 | High | Reused Credentials in Bash History File |
| 3 | High | Vulnerable WordPress Plugin (Authenticated RCE) |
| 4 | Medium | Anonymous FTP Access |
| 5 | Low | FTP Directory Listing Enabled |

### 6.2 Exploited Hosts

| Host | Scope | Method | Notes |
|------|-------|--------|-------|
| 192.168.x.10 (Ubuntu) | Internal | Sudo Privilege Abuse | `nano` shell escape → root |
| 192.168.x.20 (WIN01) | Internal | Scheduled Task Abuse | Writable task → SYSTEM |

### 6.3 Compromised Users

| Username | Host | Method | Notes |
|----------|------|--------|-------|
| john | 192.168.x.10 | SSH key + leaked creds | sudo user |
| www-data | 192.168.x.10 | WordPress plugin RCE | web server user |

### 6.4 Changes / Host Cleanup
> **Do not skip this.** Lists every artifact you left so the "client" can clean up. Graders check you tracked your own footprint.

| Host | Scope | Change / Cleanup needed |
|------|-------|-------------------------|
| 192.168.x.10 | Internal | `linpeas.sh` in `/home/john` |
| 192.168.x.20 | Internal | `shell.php` in `C:\inetpub\...` |

### 7.1 Detailed Walkthrough (the attack chain)
> Numbered so a grader can *replay it top to bottom*. Every command that mattered goes here.

| Step # | IP / Host | Action / Command | Result |
|--------|-----------|------------------|--------|
| 1.1 | 192.168.x.10 | `nmap -p- -sV 192.168.x.10 -T4 --open` | Open: 21,22,80,443 |
| 1.2 | 192.168.x.10 | `ftp 192.168.x.10` → `anonymous:anon` | Anonymous login OK |
| 1.3 | 192.168.x.10 | `get id_rsa` | Downloaded SSH private key |
| 2.1 | 192.168.x.10 | `ssh -i id_rsa john@192.168.x.10` | Foothold as `john` (user.txt) |
| 2.2 | 192.168.x.10 | `sudo -l` → `sudo nano` → `^R^X reset; sh 1>&0 2>&0` | root (root.txt) |

### 7.2 Collected Evidence
> Per step: paste the **real terminal output** in a code block, then a **screenshot** showing command + result + the target IP visible. Caption each figure with its step number.

**Screenshot rules (this is where points leak):**
- Full command AND its output in one frame.
- Target IP/hostname visible so it's clearly the exam box.
- **FLAG SCREENSHOTS ARE MANDATORY AND MUST INCLUDE `date`** — the LoE requires it explicitly:
  - Linux: `cat /home/<user>/user.txt && date` · `cat /root/root.txt && date`
  - Windows: `type C:\Users\<user>\Desktop\user.txt; date` (PowerShell `Get-Date`) — both outputs in one screenshot.
  - A flag with no `date`-stamped screenshot may not be counted, even if it's in the A.2 table.
- One clear screenshot per meaningful step beats a wall of tiny ones.

**Flag locations (per LoE):**
- USER: `/home/<user>/user.txt` (Linux) · `C:\Users\<user>\Desktop\user.txt` (Windows)
- ROOT: `/root/root.txt` (Linux) · `C:\Users\Administrator\Desktop\root.txt` (Windows)

### 8. Remediation (tie each to a Finding #)
- **Short term:** quick wins — remove the exposed `id_rsa`, disable anonymous FTP, rotate the leaked creds.
- **Medium term:** patch the WordPress plugin, enforce least-privilege sudo.
- **Long term:** password policy + rotation, periodic internal vuln assessments, admin hardening training.

### A.2 Flags Discovered

| Host | File | Flag |
|------|------|------|
| 192.168.x.10 | user.txt | `<value>` |
| 192.168.x.10 | root.txt | `<value>` |

---

## 3. Phase 2 — SIEM alert analysis (the half that ambushes people)

### ⚙️ FIRST, fix Kibana's timezone (mandatory gotcha)
> The alert timestamps in the template are in **GMT**. Kibana defaults to your **browser** timezone, so times won't match and you'll search the wrong window.
> Go to `http://<ELK_VM_IP>:5601/app/management/kibana/settings` → Advanced Settings → set **Time zone for date formatting = `Etc/GMT`**.
> Then, for each alert, **search both before AND after** the given timestamp — the alert time is a reference point, not the whole event.

### 9.4 Alert Validation table — the core deliverable

| Alert No. | True Positive | False Positive | Evidence |
|-----------|:---:|:---:|----------|
| 1 | X | | Kibana: `event.code:4624 and winlog.logon.type:10` from public IP `x.x.x.x` at 03:12 — external RDP logon, no admin change ticket. **TP.** |
| 2 | | X | `process.name:msiexec.exe` but parent is `services.exe` on patch-window schedule → legitimate software deploy. **FP.** |

**How to write an evidence cell that scores (every cell = these 4 beats):**
1. **What the alert claims** (rule / event id).
2. **The query you ran** to check it (KQL — show it literally).
3. **What the logs showed** (the field values: user, src IP, process, parent, timestamp).
4. **Verdict + one-line why.** "TP — external RDP from non-corporate IP with no change record" / "FP — expected admin activity from jump host during maintenance window."

**TP vs FP decision cues:**
- **TP signals:** foreign/public source IP, off-hours, no change ticket, suspicious parent process, base64/encoded PowerShell, new priv account, LOLBins spawning shells, C2-shaped egress, marker files (`checkme.txt`-style).
- **FP signals:** internal admin/jump host, scheduled maintenance window, known service account, benign parent (`services.exe`, `explorer.exe`), rule too broad (matches normal OS behaviour).
- **Correlation beats single events** — one 4624 is nothing; 4624 (LogonType 10, public IP) + Sysmon 3 (odd egress) + Sysmon 1 (`msiexec` spawn) in the same window = a real chain. Say so.

**KQL you'll lean on in Kibana:**
```
event.code:"4624" and winlog.event_data.LogonType:"10"     // remote interactive (RDP)
event.code:"4625"                                          // failed logon (brute force)
event.code:"4688" or event.code:"1"                        // process creation (Security / Sysmon)
event.code:"3"                                             // Sysmon network connection (C2/egress)
event.code:"11"                                            // Sysmon file create (dropped payloads)
event.code:"4720" or event.code:"4728" or event.code:"4732"// account created / added to admin group
powershell.command.value:*FromBase64String*               // encoded PowerShell
```

### `alerts.csv`
> You also submit a CSV of your verdicts. Keep a running one from hour 1 — don't reconstruct it at the end. Columns match the alert list: `alert_no, alert_name, verdict(TP/FP), evidence_summary`.

---

## 4. Workflow + submission (do the setup on Day 0/1)

1. **Get the template:** download it from the exam. If it's a `.docx`, rename to `.zip` and extract — the **alerts table (CSV)** is inside alongside the report doc.
2. **Fill the report** in the provided template (Word/LibreOffice). *Optional:* if you prefer Markdown, the `Syslifters/HackTheBox-Reporting` SysReptor template renders the same structure to PDF — but the exam-provided `.docx` is the reference; keep headings identical either way.
3. **Fill as you go, not at the end.** After each box: drop the walkthrough steps + `date`-stamped flag screenshots + evidence immediately while it's fresh. After each alert: write the verdict cell immediately and add the row to `alerts.csv`.
4. **Screenshots:** name by step (`nix01_1-2_ftp_anon.png`, `alert07_rdp_public_ip.png`) so ordering is trivial.
5. **Export to PDF early once** to confirm images embed and tables aren't broken — don't discover a render bug at hour 118.
6. **Package + submit:**
   ```bash
   zip -P hackthebox cjca_report.zip report.pdf alerts.csv
   ```
   Upload the zip in the exam dashboard. The zip **must** contain the PDF + the CSV, password `hackthebox`.

---

## 5. Pre-submit checklist (run before you export)

- [ ] Every `TODO` in the template replaced or removed.
- [ ] Candidate name / client details filled (sec 2, 4).
- [ ] Findings table severities match the impact you demonstrated.
- [ ] Every flag in A.2 has a walkthrough step that produced it.
- [ ] 7.1 step table is replayable top-to-bottom with no gaps.
- [ ] Every walkthrough step has evidence (output block + screenshot) in 7.2.
- [ ] Screenshots show command + result + target IP.
- [ ] **Every flag has a screenshot with the `date` command in-frame** (`cat ... && date`).
- [ ] Cleanup section (6.4) lists every artifact you dropped.
- [ ] Remediation items each reference a Finding #.
- [ ] **≥80 Phase-1 points** worth of flags documented + reproducible.
- [ ] **≥27 alerts** classified with a 4-beat evidence cell each.
- [ ] `alerts.csv` verdicts match the report table exactly (`true`/`false` per alert).
- [ ] PDF rendered clean — images embedded, tables intact, page breaks sane.
- [ ] Final zip built: `zip -P hackthebox cjca_report.zip report.pdf alerts.csv` (PDF + CSV inside, password `hackthebox`).

---

## References
- Official template: `https://docs.sysreptor.com/assets/reports/HTB-CJCA-Report.pdf`
- SysReptor HTB templates: `https://github.com/Syslifters/HackTheBox-Reporting`
- Related: [[Example of an incident Analysis and response report]] · [[Reporting]] · [[Proof-of-Concept]]
