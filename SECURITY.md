# Security Policy

This repository contains **documentation only** — architecture write-ups, design rationale, and roadmap notes about a personal homelab. It does not contain source code that is deployed anywhere, and it is not intended to contain credentials, tokens, real network addressing, or any other operational secret.

## Scope

This policy covers the contents of this repository (documentation, diagrams, and any scripts that may be added later) — not the private infrastructure it describes. The lab itself is not internet-facing and is out of scope for testing.

## Reporting an issue

If you find something in this repo that shouldn't be public — for example, an accidentally committed secret, a real IP address, or any other identifying detail that slipped past redaction — please report it privately rather than opening a public issue:

1. Open a [private security advisory](../../security/advisories/new) on this repository, **or**
2. Email the maintainer directly (see the GitHub profile for contact info).

Please include the file and line in question. I'll aim to acknowledge within a few days and remove/rotate anything sensitive as soon as possible, including scrubbing it from git history if needed.

## Out of scope

General suggestions, typos, or architecture feedback are welcome as normal public issues or pull requests — this policy is specifically for anything sensitive that shouldn't sit in a public issue tracker.
