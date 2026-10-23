# id 1 — Mini Ledger API

- **Points:** 68 (final; initial 82 — values decay as more teams solve)
- **Category:** Web / API
- **Status:** ✅ Solved

## Vulnerability
Broken access control on the money-transfer endpoint. The API trusts a client-supplied `from` account without checking that the caller owns it, so any account can be named as the source of a transfer.

## Exploitation
1. Found the transfer endpoint accepted a JSON body naming both the source and destination account.
2. Issued a transfer drawing from an account we do not own:
   ```
   POST /api/transfer
   {"from":"vault","to":"you","amount":1500000}
   ```
3. With the balance moved, the flag endpoint released the flag:
   ```
   GET /api/flag
   ```

## Flag
`flag{79f2d263-ce03-4654-bb35-854fcd07ca3e}`

## Lessons
- Never trust a client-supplied source account. Authorize every transfer against the authenticated identity, not a field in the request body.
