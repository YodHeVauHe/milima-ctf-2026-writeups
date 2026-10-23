# id 18 — Member Directory

- **Points:** 194 (final; initial ~222 — values decay as more teams solve)
- **Category:** Web / SQL injection
- **Status:** ✅ Solved

## Vulnerability
SQL injection in the `/search` endpoint of the member directory app.

## Exploitation
1. Injected on the `/search` endpoint to dump the `staff_credentials` table.
2. The manager account's password was stored as an **unsalted MD5**.
3. Cracked the MD5 to recover the plaintext password `sacco123`.
4. Logged in as the manager, which revealed the flag.

## Flag
Submitted & accepted (value not recorded in notes).

## Lessons
- Parameterize queries; never concatenate user input into SQL.
- Never store passwords as unsalted MD5 — a single cracked hash gave full account access.
