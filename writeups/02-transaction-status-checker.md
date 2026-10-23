# id 2 — Transaction Status Checker

- **Points:** 310
- **Category:** Web / boolean-blind SQL injection (SQLite backend)
- **Status:** ✅ Solved

## Vulnerability
Boolean-blind SQL injection in a status-lookup endpoint over a SQLite backend.

## Exploitation
1. Endpoint `GET /check?ref=<value>` returns a JSON oracle: `{"result":"Found"}` vs `{"result":"Not found"}`.
2. The weak input filter blocks only the literal strings `union` and `--`; nothing else.
3. The injection is inside a single-quoted string; used a quote-balancing payload: `x' OR (<CONDITION>) AND 'a'='a`.
4. Dumped the flag from a `secrets` table **char-by-char via binary search** — `length()` to find the length, then `unicode(substr(col,pos,1))` per position — automated with a short Python oracle script.

## Flag
`flag{7d2b1002-6b0b-462d-8af6-8cc222f47a2e}`

## Lessons
- Blacklist filters (blocking only `union`/`--`) are trivially bypassed; use parameterized queries.
- A boolean oracle is enough to exfiltrate any data one bit at a time.
