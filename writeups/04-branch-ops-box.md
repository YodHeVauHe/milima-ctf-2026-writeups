# id 4 — Branch Ops Box

- **Points:** 352
- **Category:** SMB / SNMP
- **Status:** ❌ Unsolved (attempted)

## Target
SMB on `185.158.107.222:21712`.

## Where we got
Null-session `rpcclient` revealed the share-owning user **`svc-backup`**. The `backup` share requires credentials to access.

## What blocked us
- The `backup` share credentials were never cracked. `sysadmin:RpWxPmkCZrLLWW` (reused from id 5) returned **`LOGON_FAILURE`**.
- The intended credential source was a "management service" **SNMP on UDP `216NN`**, which was confirmed **dead/unreachable** — a hard UDP timeout from **both our box and the user's on-site network** (tested `public`, `private`, and v1).

## Intended path
Pull `svc-backup`'s credentials from the SNMP management service, then authenticate to the `backup` SMB share to read the flag. Blocked — SNMP answered nothing. No fabricated flag.
