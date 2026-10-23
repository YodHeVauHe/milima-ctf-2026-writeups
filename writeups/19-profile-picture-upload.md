# id 19 — Profile Picture Upload

- **Points:** 222
- **Category:** Web / file upload (`dynamic_docker`)
- **Status:** ❌ Unsolved (bug found, flag not captured)

## Where we got — bug proven
The "is it an image?" check trusts **only the multipart `Content-Type` header** (attacker-controlled), not the file extension or the file bytes. This means arbitrary HTML/JS can be stored as a "picture." Uploaded files are saved as `{random16hex}-{basename}` in a dedicated directory, and `/preview/<name>` reads that directory and serves the bytes back as `text/html`.

## Confirmed dead ends
- No path traversal / LFI — basename is enforced on both save and read.
- No SSTI / template execution — content is served verbatim.
- No directory listing and no direct storage URL.
- No `/flag` or admin/bot route.
- The CSPRNG filename prefix is unpredictable; ~**5,700** brute-forced flag/metadata filenames all returned 404.

## What blocked us
The flag file could not be located black-box. The missing piece is the flag's on-disk name/location in the save directory, which would require the challenge source.

## Note
No flag was captured. Nothing is fabricated here.
