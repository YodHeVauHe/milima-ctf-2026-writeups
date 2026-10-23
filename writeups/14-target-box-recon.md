# id 14 — Target Box Recon

- **Points:** 96
- **Category:** Recon / weak-credential brute-force
- **Status:** ❌ Unsolved (attempted)

## Target
SSH `studentNN@185.158.107.222:21NN` (FTP on `22NN`).

## Where we got
Weak-credential brute-force: ran rockyou slices `0–45k` over both SSH and FTP.

## What blocked us
FTP was **throttled (~230/min per IP by vsftpd)**, so only roughly the first **29k of rockyou** was reached by the cutoff. No hit — the password is deeper in rockyou than the time allowed.

## Intended path
Brute-force the weak `student` credential, then use it as the foothold feeding **Privilege Escalation (id 13)**. Not cracked — no fabricated flag.
