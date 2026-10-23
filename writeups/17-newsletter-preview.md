# id 17 — Newsletter Preview

- **Points:** 190 (final; initial 218 — values decay as more teams solve)
- **Category:** Web / SSTI
- **Status:** ✅ Solved

## Vulnerability
Server-Side Template Injection (SSTI) in a Flask/Jinja2 application. User input to the newsletter "preview" (the greeting that renders as `Hello, <name>!`) was placed directly into a server-side template and evaluated.

## Exploitation
1. Confirmed template evaluation by submitting a template expression in the name/preview field and seeing it rendered server-side.
2. Dumped the Flask config and environment through the injected context, e.g. `{{ config }}` and the application's `os.environ`, which exposed the flag as the `FLAG` environment variable.
3. Equivalently, read it straight off disk via the template context (`/flag.txt`), which returned `Hello, flag{...}! Thanks for being a SACCO member`.

## Flag
`flag{92d5ebba-3666-4db2-898f-616afeba61cb}`

## Lessons
- Never render untrusted input through a server-side template engine. Use autoescaping context and pass user data as template variables, not as template source.
- Keep secrets out of environment variables reachable from the request context.
