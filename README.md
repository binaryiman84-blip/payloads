# payloads/

A categorized collection of attack payloads used during recon and active testing (pairs with Prorecon.sh and 4-ZERO-3). Organized by vulnerability class so individual scripts can pull a single wordlist without parsing an unrelated one.

## Structure

```
payloads/
├── xss/
│   ├── reflected.txt
│   ├── dom.txt
│   └── polyglot.txt
├── sqli/
│   ├── error-based.txt
│   ├── blind-boolean.txt
│   ├── blind-time.txt
│   └── union.txt
├── ssrf/
│   ├── internal-ips.txt
│   ├── cloud-metadata.txt
│   └── bypass-encodings.txt
├── lfi/
│   ├── path-traversal.txt
│   └── wrapper-payloads.txt
├── cors/
│   └── origin-bypass.txt
├── 403-bypass/
│   ├── headers.txt
│   └── path-mutations.txt
├── open-redirect/
│   └── redirect-payloads.txt
├── ssti/
│   └── engine-specific.txt
├── xxe/
│   └── entity-payloads.txt
└── README.md
```

## Usage

Each file is plain text, one payload per line, consumable by tools like `ffuf`, `dalfox`, `sqlmap -m`, or custom `xargs -P` loops.

```bash
# Example: feed XSS payloads into a parameter fuzzer
ffuf -w payloads/xss/reflected.txt -u 'https://target.tld/search?q=FUZZ'

# Example: time-based SQLi sweep
sqlmap -m targets.txt --technique=T --tamper=space2comment
```

## Conventions

- **Lowercase, hyphenated filenames** — no spaces.
- **One payload per line**, no trailing whitespace, no comments inline (keeps files pipeable).
- **No destructive payloads** (no `DROP`, no filesystem-write primitives) — this set is for detection, not exploitation.
- Payloads are **context-agnostic**; URL-encode/double-encode at call time rather than baking encoding into the file, unless the file is explicitly named `*-encodings.txt` or `*-bypass.txt`.

## Updating

When adding a new category:
1. Create the subdirectory under `payloads/`.
2. Add a short comment block at the top of this README's Structure section.
3. Keep each file under ~500 lines; split by sub-technique (e.g. `blind-boolean.txt` vs `blind-time.txt`) rather than growing one giant list.

## Sources

Payloads are pulled/adapted from PayloadsAllTheThings, SecLists, and manual findings logged during active engagements. Attribute non-original entries in a trailing comment in the relevant source repo, not in these files.

## Disclaimer

For use only against in-scope targets under an active bug bounty or authorized engagement. Verify program scope before firing any payload set.
