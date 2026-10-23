# id 7 — Portal Under Construction

- **Points:** 66 (final; initial 82)
- **Category:** Web / exposed `.git`
- **Status:** ✅ Solved

## Vulnerability
An exposed `.git` directory in the web root allowed full source recovery — including a secret in a file that had been deleted but was still in git history.

## Exploitation
1. Detected the exposed `.git` directory.
2. Dumped the repository with **git-dumper**.
3. Recovered a **deleted** `app/config.py` from git history; it contained `SECRET_FLAG`.

## Flag
`flag{b4caefef-549b-4fbb-8251-30fa616b896c}`

## Lessons
- Never deploy `.git` to a web root. Deleting a secret from the working tree does not remove it from history.
