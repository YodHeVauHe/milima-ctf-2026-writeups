# id 15 — HR File Server

- **Points:** 218
- **Category:** SMB enumeration (whale container)
- **Status:** ✅ Solved

## Vulnerability
Sensitive info leaked in an account's SMB *Description* field, plus a hidden (non-browseable) share.

## Exploitation
1. Anonymous SMB enumeration (`smbclient -L //HOST -p PORT -N`) showed only a decoy `public` share.
2. `rpcclient -U "%" HOST -p PORT -c "querydispinfo"` leaked user **jkamau**'s account *Description*, which contained a temp password: `Karibu8894`.
3. The flag lived on a **hidden, non-browseable** share named `hr-confidential`, found by brute-forcing share names and authenticating as `jkamau:Karibu8894`.

## Flag
`flag{e50d37ad-5632-4cb5-b8f0-28dcbc5f20af}`

## Lessons
- `querydispinfo` exposes account description fields even to null sessions — never put secrets there.
- "Hidden" shares are security-by-obscurity; they are trivially enumerable.
