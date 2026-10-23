# id 8 — Payroll Audit Dump

- **Points:** 400
- **Category:** Web / crypto (`dynamic_docker`)
- **Status:** ❌ Unsolved (attempted)

## Where we got
Recon found a hidden `/exports/leaked_hashes.txt` (the intended first step — nothing on the portal links to it). It contained 4 hashes:

| Account | Hash |
|---|---|
| admin | `9a4153d6a5fc673087df7c4b51d3c5ee` |
| jkamau | `182705339546264ae8cda5d21cd56b45` |
| finance_svc | `7c0dd23ee06c6f9c2c6d18cdd5b343da` |
| mnjoroge | `b0a46dfbbcf080b58974667c7da85350` |

## What blocked us
These were **not crackable** as raw MD5 or NTLM against full rockyou, best64 rules, or a themed word+digits hybrid (all confirmed with hashcat) — they are **custom passwords**. The sign-in was explicitly "not the finish line" — a post-auth chain remained beyond it.

In the final minutes the challenge was also `dynamic_docker` and its container could not be booted (platform-wide 403).

## Intended path
Crack a leaked hash → sign in → complete a post-authentication chain to reach the flag. Blocked at the hash-crack step. No fabricated flag.
