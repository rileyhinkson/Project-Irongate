# Project IronGate

A self-hosted AI development and cybersecurity homelab, documented publicly as a running record of my growth as an infrastructure/security architect.

This repo isn't marketing copy — it's the actual design log for a lab I run: a small fleet of purpose-built nodes running local LLM inference, a private dev workflow, a defensive security stack, and an isolated offensive-security range for hands-on skills work. I'm sharing it so employers, other developers, and fellow homelab/architecture folks can see how I think about design tradeoffs, segmentation, and cost — not just the end result.

## Why this exists

I wanted a place to show *how* a system evolves, not just a finished diagram. Expect this repo to change as the lab changes: new hardware, new roadmap milestones, lessons learned when something breaks. Think of it as a living architecture decision record.

## A note on what's deliberately not here

This is a real, currently-running personal network, so it's written and maintained with a simple rule: **publish the design, not the blueprint.**

That means:
- No physical location, ISP, or public IP information.
- No real IP addressing, VLAN numbering, switch port maps, or device configuration exports.
- No credentials, API keys, hostnames-to-address mappings, or remote-access details.

What you *will* find is the actual design reasoning: how the fleet is segmented, why nodes are split the way they are, what the security stack looks like, and what's planned next. If you're an employer or collaborator who wants to go deeper on the technical specifics, I'm glad to walk through it live — reach out.

## Fleet snapshot

| | |
|---|---|
| Nodes (active) | 9 rack/infra nodes + always-on client devices |
| Network segmentation | 6 purpose-built VLANs, default-deny between them |
| Security posture | SIEM, IDS/IPS, secrets management, CI security gates, isolated offensive-security lab |
| Local AI | Self-hosted LLM inference for daily dev work, zero cloud dependency for routine tasks |
| Est. operating cost | ~$24/month in electricity (duty-cycled) |
| Status | Actively evolving — see the [roadmap](docs/ROADMAP.md) |

## Contents

- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — design principles, fleet roster, node roles, storage strategy
- [`docs/NETWORK.md`](docs/NETWORK.md) — segmentation model and firewall policy (redacted of addressing)
- [`docs/CYBERSECURITY.md`](docs/CYBERSECURITY.md) — defensive stack, isolated offensive lab, AI-assisted SecOps
- [`docs/ROADMAP.md`](docs/ROADMAP.md) — power/cost model and the 6-month through 5-year plan
- [`SECURITY.md`](SECURITY.md) — how to report an issue with this repo's contents

## License

Documentation and any accompanying scripts in this repo are provided under the [MIT License](LICENSE).
