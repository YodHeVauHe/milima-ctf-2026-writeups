# id 13 — Privilege Escalation

- **Points:** 96
- **Category:** Pwn / privilege escalation
- **Status:** ❌ Unsolved (attempted)

## Target
Same box as **Target Box Recon (id 14)**: `studentNN @ 185.158.107.222:21NN`.

## Where we got
Needed the `student` account's SSH password first (the foothold from id 14), then a local root escalation from there.

## What blocked us
The foothold password was **never cracked** (see id 14 — it sits deeper in rockyou than the clock allowed, and FTP throttling capped how much of the wordlist we could test). Without the initial SSH login there was no shell to escalate from.

## Intended path
Obtain the `student` SSH foothold, then perform a local privilege escalation to root to read the flag. Not reached — no fabricated flag.
