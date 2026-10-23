# id 20 — Feedback Wall

- **Points:** 194 (final; initial 222 — values decay as more teams solve)
- **Category:** Web / XSS (admin-bot)
- **Status:** ✅ Solved

## Vulnerability
Reflected Cross-Site Scripting (XSS) combined with an admin review bot. The feedback message was reflected unescaped via `/?msg=`, and an automated admin "reviewer" visited submitted feedback links — so a crafted link ran attacker JavaScript in the admin's authenticated session.

## Exploitation
1. Confirmed `/?msg=` reflected input into the page without escaping.
2. Built a payload-bearing URL that, when opened, exfiltrated the admin-only data to a collection endpoint.
3. Submitted that URL through the feedback path so the admin bot visited it.
4. Retrieved the captured result from `/collected`, which returned the flag (URL-encoded as `{"flag":"flag{...}"}`).

## Flag
`flag{d1f8ec29-f578-40fa-a69c-0cdedeae7a7c}`

## Lessons
- Escape all reflected output and set a strict Content-Security-Policy.
- An admin bot that auto-visits user links turns reflected XSS into full account takeover.
