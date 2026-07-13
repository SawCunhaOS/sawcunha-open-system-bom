# Security Policy

## Supported versions

| Version | Supported          |
|---------|--------------------|
| 1.3.x   | :white_check_mark: |
| < 1.3   | :x:                |

Only the latest minor line receives security updates. As a BOM, most fixes are delivered as dependency
version bumps.

## Reporting a vulnerability

**Do not open a public issue for security problems.**

Report privately through either channel:

1. **GitHub Private Vulnerability Reporting** (preferred) — on this repository, go to the **Security** tab →
   **Report a vulnerability**. This opens a private advisory visible only to maintainers.
2. **Email** — `community@sawcunha.io` with subject `[SECURITY] scos-bom`.

Please include:

- affected version(s) or coordinates,
- the vulnerable dependency (BOM issues are usually a managed version),
- a description and, if possible, a reproduction or a link to the upstream advisory (CVE/GHSA),
- the impact you foresee.

## What to expect

- **Acknowledgement** within 5 business days.
- An assessment and, where applicable, a fix delivered as a version bump on the supported line.
- **Coordinated disclosure**: we credit reporters (unless you prefer to remain anonymous) and publish a
  GitHub Security Advisory once a fix is available.

## Scope

This repository ships no runtime code — it manages dependency versions and build configuration.
Vulnerabilities therefore typically concern a **managed dependency version** (raise it) or the **build/CI**
configuration. Issues in the dependencies themselves should also be reported upstream to their maintainers.
