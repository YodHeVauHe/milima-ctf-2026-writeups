# id 9 — Member Statements

- **Points:** 74 (final; initial 88 — values decay as more teams solve)
- **Category:** Web
- **Status:** ✅ Solved

## Vulnerability
Insecure Direct Object Reference (IDOR). The account-statement view took an `account_id` from the query string and returned that account's statement without checking it belonged to the logged-in member.

## Exploitation
1. Logged in with the demo member account `member : member123`.
2. Noted the statement page loaded via `GET /statement?account_id=<own id>`.
3. Walked the `account_id` parameter to another account:
   ```
   GET /statement?account_id=1042
   ```
   which returned the statement for **"Branch Manager - Reserve Account"** (balance KES 8,750,000).
4. The flag was contained in that privileged account's statement.

## Flag
`flag{74bfa779-6eb0-4e26-9bf0-d7983953c09d}`

## Lessons
- Always scope object lookups to the authenticated user; never trust an ID from the request.
