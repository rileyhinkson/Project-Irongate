# Cybersecurity Stack

IronGate runs two missions side by side: a **defensive** posture protecting the production lab, and an **isolated offensive** range for hands-on skills work. They're kept deliberately separate at the network level (see [`NETWORK.md`](NETWORK.md)) and fused only at the analysis layer.

## Defensive (blue-team, production)

- **Network security monitoring** — inline IDS/IPS at the network edge with a maintained ruleset; alerts flow into the SIEM rather than living in an isolated log.
- **SIEM / log correlation** — centralized log collection and file-integrity monitoring across every Linux node, with CVE-aware detection rules.
- **Secrets management** — a self-hosted vault for personal credentials, separate from pipeline secrets management for CI/CD.
- **CI security gates** — every commit through the private CI pipeline runs secret scanning, static analysis (SAST), and dependency/container vulnerability scanning before anything ships.
- **Vulnerability management** — periodic authenticated network scans against the lab's own infrastructure, run on demand rather than continuously (this is a homelab, not a production SOC — cadence is tuned accordingly).

## Offensive (isolated lab)

A dedicated offensive-security OS install runs on the inference workstation's secondary drive. Vulnerable-by-design targets run as VMs pinned to the isolated lab zone with no egress — no path to production, no path to the internet unless explicitly and temporarily opened for a specific exercise. A separate no-network throwaway VM exists for safe malware-analysis practice.

## AI-assisted SecOps — where the two missions fuse

The same local LLM infrastructure that powers day-to-day development is pointed at security work too:

- IDS/SIEM alerts get triaged and summarized in plain language by a local model — a first-pass analyst that never leaves the network.
- The coding model reviews commits in CI and flags likely vulnerabilities before they land.
- Retrieval-augmented generation runs over CVE data and the lab's own (sanitized) configuration knowledge base — a private analyst that never phones home.

## Why this exists beyond the homelab

This stack maps directly onto real SOC-analyst domains — SIEM operation, IDS/IPS tuning, vulnerability management, incident triage — which makes running the lab directly relevant practice for Security+ / CySA+ certification work, not just a hobby project.

## Responsible use

The offensive-security lab exists purely for authorized, self-directed skills practice against intentionally vulnerable targets that I own and control. It has no route to production and no route to any third party's infrastructure.
