# id 12 — Account Recovery

- **Points:** 206 (final; initial 238)
- **Category:** Web / broken authentication (predictable token)
- **Status:** ✅ Solved

## Vulnerability
The password-reset token was `md5(username + unix_second_the_token_was_issued)` — derived entirely from public/guessable values with **no server-side secret**.

## Exploitation
1. Read the server's current clock from victim `j.mukasa`'s own reset page.
2. Computed/forged the reset token for Branch Manager **Grace Nakato** (username `nakatog`) as `md5("nakatog" + <unix_second>)`.
3. Used the forged token to take over Grace Nakato's account and reveal the flag.

## Flag
`flag{690f8cd8-7d56-41e5-a5fa-286b7ef9d09d}`

## Lessons
- Password-reset tokens must be high-entropy, random, and server-secret-bound — never a hash of predictable inputs.
