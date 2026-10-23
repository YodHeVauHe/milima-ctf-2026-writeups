# id 11 — Ops Box

- **Points:** 72
- **Category:** Pwn / Linux privilege escalation (whale container)
- **Status:** ✅ Solved

## Vulnerability
A world-writable script that is executed as root by cron.

## Exploitation
1. Booted the whale container and connected to the assigned SSH/nc `host:port`.
2. Logged in as `opsuser:letmein2024`.
3. Found `/opt/scripts/backup-check.sh` was **world-writable** and run as root every minute via `/etc/cron.d/backup-check`.
4. Overwrote the script to copy `/root/flag.txt` to a world-readable path (`/tmp/rf.txt`).
5. Waited for the cron run, then read `/tmp/rf.txt`.

## Flag
`flag{d643d8a6-d01f-437b-9d73-75d2555ce524}`

## Lessons
- Root-run cron scripts must never be writable by unprivileged users.
