# id 6 — NetOps Console

- **Points:** 66 (final; initial 80 — values decay as more teams solve)
- **Category:** Web / Network
- **Status:** ✅ Solved

## Vulnerability
Default/weak administrative credentials on the router-style management console. The login accepted a well-known default pair.

## Exploitation
1. Reached the console login page.
2. Tried common default router credentials and `admin : admin123` succeeded (HTTP 200).
3. The authenticated console page returned the flag directly in its body.

## Flag
`flag{9e8fa27e-62b4-4c9e-980e-743526c27b47}`

## Lessons
- Force a password change on first login and never ship default credentials.
- Rate-limit and alert on login attempts against management interfaces.
