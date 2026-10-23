# Milima CTF Arena 2026 — Write-ups (Team ZaSourceCode)

Complete write-ups for **Milima CTF Arena**, held at the EastCon / CyberSecurity Symposium 2026.

## Event overview

| | |
|---|---|
| **Event** | Milima CTF Arena — EastCon / CyberSecurity Symposium 2026 |
| **Venue** | National ICT Innovation Hub, Kampala |
| **When** | Friday, Oct 23, 2026, 9:00am–4:10pm EAT |
| **Platform** | CTFd + `ctfd-whale` (per-team single running container; dynamic Docker challenges) |
| **Static target host** | `185.158.107.222` |

## Team & final standing

| | |
|---|---|
| **Team** | ZaSourceCode (team id 108) |
| **Captain** | ZaSourceCode (mugharold@gmail.com) |
| **Final score** | **2686 points** |
| **Place** | **7th** |
| **Challenges solved** | 15 of 20 |

> Points are **dynamic** — a challenge's value decays as more teams solve it. The values in the table below are the **final** values at the close of the event. Where an initial value was recorded, both are shown in the challenge's write-up.

## All challenges

| ID | Challenge | Points | Category / Type | Status | Vulnerability |
|---|---|---|---|---|---|
| 1 | [Mini Ledger API](writeups/01-mini-ledger-api.md) | 68 | Web / API | ✅ Solved | Exploit details not recorded in our notes |
| 2 | [Transaction Status Checker](writeups/02-transaction-status-checker.md) | 310 | Web / blind SQLi | ✅ Solved | Boolean-blind SQL injection (SQLite) |
| 3 | [Payment Ops Console](writeups/03-payment-ops-console.md) | 316 | Web / RCE | ✅ Solved | OS command injection + sudo (`env`) privesc |
| 4 | [Branch Ops Box](writeups/04-branch-ops-box.md) | 352 | SMB / SNMP | ❌ Unsolved | Backup-share creds uncracked; SNMP cred source dead |
| 5 | [Branch Web Host](writeups/05-branch-web-host.md) | 198 | Web → SSH | ✅ Solved | Directory listing exposed a backup file leaking SSH creds |
| 6 | [NetOps Console](writeups/06-netops-console.md) | 66 | Web/Network | ✅ Solved | Exploit details not recorded in our notes |
| 7 | [Portal Under Construction](writeups/07-portal-under-construction.md) | 66 | Web / exposed .git | ✅ Solved | Exposed `.git` dir; secret in a deleted committed file |
| 8 | [Payroll Audit Dump](writeups/08-payroll-audit-dump.md) | 400 | Web / crypto | ❌ Unsolved | Found leaked hashes; custom passwords uncrackable in time |
| 9 | [Member Statements](writeups/09-member-statements.md) | 74 | Web | ✅ Solved | Exploit details not recorded in our notes |
| 10 | [Ledger API Auth](writeups/10-ledger-api-auth.md) | 70 | Web / API auth | ✅ Solved | Exploit details not recorded in our notes |
| 11 | [Ops Box](writeups/11-ops-box.md) | 72 | Pwn / Linux privesc | ✅ Solved | World-writable root-run cron script |
| 12 | [Account Recovery](writeups/12-account-recovery.md) | 206 | Web / broken auth | ✅ Solved | Predictable password-reset token (`md5(user+epoch)`) |
| 13 | [Privilege Escalation](writeups/13-privilege-escalation.md) | 96 | Pwn / privesc | ❌ Unsolved | Gated on uncracked foothold password (see id 14) |
| 14 | [Target Box Recon](writeups/14-target-box-recon.md) | 96 | Recon / brute-force | ❌ Unsolved | Weak-cred brute; FTP throttling capped rockyou coverage |
| 15 | [HR File Server](writeups/15-hr-file-server.md) | 218 | SMB enumeration | ✅ Solved | Password leaked in account description; hidden share |
| 16 | [Link Preview Tool](writeups/16-link-preview-tool.md) | 226 | Web | ✅ Solved | Exploit details not recorded in our notes |
| 17 | [Newsletter Preview](writeups/17-newsletter-preview.md) | 190 | Web | ✅ Solved | Exploit details not recorded in our notes |
| 18 | [Member Directory](writeups/18-member-directory.md) | 194 | Web / SQLi | ✅ Solved | SQL injection in `/search`; cracked unsalted MD5 |
| 19 | [Profile Picture Upload](writeups/19-profile-picture-upload.md) | 222 | Web / file upload | ❌ Unsolved | Content-Type-only image check bypassed; flag file not located |
| 20 | [Feedback Wall](writeups/20-feedback-wall.md) | 194 | Web | ✅ Solved | Exploit details not recorded in our notes |

## Highlights

- **HR File Server (id 15)** — Anonymous SMB gave only a decoy share, but `rpcclient querydispinfo` leaked a temp password buried in a user's account *Description* field. The flag sat on a **hidden, non-browseable share** found by brute-forcing share names.
- **Account Recovery (id 12)** — The password-reset token was `md5(username + unix_second_issued)` with **no server secret**. Reading the server clock off the victim's own reset page let us forge a branch manager's reset token and take over the account.
- **Portal Under Construction (id 7)** — An exposed `.git` directory was dumped with `git-dumper`; the secret lived in a `config.py` that had been **deleted** but was still recoverable from git history.
- **Member Directory (id 18)** — Classic SQLi → dump `staff_credentials` → crack an **unsalted MD5** (`sacco123`) → log in as the manager.

## Tooling & methodology

- **CTFd API via `curl`** — all challenge detail fetches and flag submissions went through the CTFd REST API (`/api/v1/challenges/<id>`, `/api/v1/challenges/attempt`, `/api/v1/teams/me`) with a cookie jar and the CSRF nonce.
- **`smbclient` / `rpcclient`** — anonymous share enumeration, user enumeration, and the `querydispinfo` description leak.
- **Custom SMB script** (`smbprotocol`) — per-attempt fresh `Connection`+`Session` SMB authentication for share-name and credential brute-forcing (the high-level session-caching API produced false negatives).
- **`hashcat`** — raw MD5 (`-m 0`) and NTLM (`-m 1000`), `best64` rules, and word+digits hybrid (`-a 6`) attacks.
- **`git-dumper`** — reconstructing the repo from an exposed `.git` directory, including deleted files from history.
- **`hydra`** — SSH/FTP weak-credential brute-forcing (rockyou slices).
- **Custom SNMP script** — attempting to reach the "management service" SNMP on UDP for Branch Ops Box.

## Unsolved — how far we got

- **id 4 Branch Ops Box (352)** — Null-session `rpcclient` revealed the share-owning user `svc-backup`; the `backup` share needed credentials we never cracked (`sysadmin:RpWxPmkCZrLLWW` → `LOGON_FAILURE`). The intended credential source — a "management service" SNMP on UDP `216NN` — was **dead/unreachable** (hard UDP timeout) from both our box and the user's on-site network.
- **id 8 Payroll Audit Dump (400)** — Recon found a hidden `/exports/leaked_hashes.txt` (the intended first step). The 4 hashes were **not** crackable as raw MD5/NTLM against full rockyou + best64 + a themed hybrid — they are custom passwords. A post-auth chain remained beyond the sign-in. Also `dynamic_docker`, un-bootable in the final minutes (platform-wide 403).
- **id 13 Privilege Escalation (96)** & **id 14 Target Box Recon (96)** — The whole chain gated on an SSH foothold password that sits **deeper in rockyou than the clock allowed**; FTP was throttled to ~230/min per IP so only ~29k of rockyou was reached.
- **id 19 Profile Picture Upload (222)** — We proved the bug (the image check trusts only the attacker-controlled multipart `Content-Type`, so arbitrary HTML/JS can be stored and served back via `/preview/<name>`) but could not locate the flag file's on-disk name black-box (CSPRNG filename prefix; ~5,700 brute-forced names 404'd). No fabricated flag.

See each challenge's file for the full "how far we got / what blocked us / intended path".
