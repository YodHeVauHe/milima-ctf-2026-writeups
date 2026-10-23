# id 10 — Ledger API Auth

- **Points:** 70 (final; initial 80 — values decay as more teams solve)
- **Category:** Web / API auth
- **Status:** ✅ Solved

## Vulnerability
JWT `alg:none` acceptance. The API issued HS256 tokens but the verifier also accepted tokens with the algorithm set to `none` and an empty signature, allowing full token forgery.

## Exploitation
1. Logged in with the demo account `alice : alice123` and received an HS256 JWT with payload `{"user":"alice","role":"user"}`.
2. Confirmed the verifier accepted an unsigned token by forging a new JWT:
   - Header: `{"alg":"none","typ":"JWT"}`
   - Payload: `{"user":"alice","role":"admin"}`
   - Signature: empty
3. Presented the forged admin token to the privileged endpoint `/admin`, which paid out the flag.

## Flag
`flag{ae133b91-f668-4a5e-abf6-e081f53cef99}`

## Lessons
- Pin the expected algorithm server-side and reject `none` outright.
- Treat the token's claims as untrusted until the signature verifies under the server's own key.
