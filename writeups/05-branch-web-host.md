# id 5 — Branch Web Host

- **Points:** 198
- **Category:** Web → SSH
- **Status:** ✅ Solved

## Vulnerability
Directory listing enabled on the web host, exposing a backup file that leaked SSH credentials.

## Exploitation
1. The web host (port `21412` on `185.158.107.222`) had **directory listing** enabled.
2. Browsing exposed `/backup/sysadmin_creds.txt.bak`, which leaked SSH credentials: `sysadmin:RpWxPmkCZrLLWW`.
3. SSH'd into the box with those credentials and read the flag.

## Flag
`FLAG{b9e25aa19c318f5c}`

## Lessons
- Disable directory listing; never ship `.bak`/backup files in a web root.
