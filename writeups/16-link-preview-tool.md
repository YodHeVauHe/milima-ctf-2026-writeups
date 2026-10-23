# id 16 — Link Preview Tool

- **Points:** 226 (final; initial 242 — values decay as more teams solve)
- **Category:** Web / SSRF
- **Status:** ✅ Solved

## Vulnerability
Server-Side Request Forgery (SSRF). The "Link Preview" feature fetched a user-supplied URL server-side to render a preview, with no restriction on scheme or destination. That outbound fetch could be pointed at resources the server could reach but the client could not (internal/local endpoints and, consistent with the other read-the-flag challenges in this set, a local flag resource).

## Exploitation
The challenge was solved and the flag banked during the event (confirmed in the team solve log, banked **09:59 EAT**) via the SSRF: the preview fetcher was aimed at an internal/local resource to retrieve the flag.

> **Note on completeness:** this challenge was solved in a separate working session whose step-by-step log was not preserved, so the exact payload and the exact flag string are not recoverable from our notes. Rather than fabricate them, we record only what is verified: the challenge is a Link Preview SSRF, it was solved, and the flag was accepted on the platform. Everything else in this file is grounded in that confirmed record.

## Flag
Submitted & accepted on the CTFd platform. **Exact flag string not preserved in our notes** (not reconstructable after the event — the challenge detail API is locked once the event ends).

## Lessons
- Validate and allow-list outbound fetch targets; block internal ranges, `localhost`, link-local (`169.254.169.254`), and non-HTTP schemes such as `file://`.
- Record the exact payload and flag at solve time — a per-team dynamic flag cannot be re-derived after the event closes.
