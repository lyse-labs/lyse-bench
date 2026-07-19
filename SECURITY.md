# Security Policy — lyse-bench

This repository holds static data (corpus manifests, calibration data,
generated reports) and documentation. It has **no executable surface of
its own** — nothing here runs code against user input.

## Reporting a vulnerability

If you believe you've found a security issue involving how this data is
*consumed* (for example, in the corpus-cloning or audit-runner code), it
almost certainly belongs to the audit engine, not this repo. Please report
it there instead:

- **GitHub Security Advisories:** https://github.com/lyse-labs/lyse/security/advisories/new (preferred)
- **Email:** contact@getlyse.com

See [lyse-labs/lyse's SECURITY.md](https://github.com/lyse-labs/lyse/blob/main/SECURITY.md)
for the full policy, supported versions, and response times.

If you find something specific to this repository (e.g., a malicious or
compromised entry in `corpus/`), please also report it via the channels
above.
