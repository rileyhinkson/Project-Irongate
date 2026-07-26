# Power, Cost & Roadmap

## Power & electricity

Modeled scenarios for the fleet's wall-load draw, from idle to full concurrent load:

| Scenario | Notes |
|---|---|
| Idle floor (24/7 nodes only, idle) | Absolute minimum draw |
| Typical floor (24/7 nodes, average use) | Normal always-on baseline |
| Active dev/inference session | Floor + inference workstation at full load |
| Nightly backup window | Floor + backup node awake, inference workstation off |
| Absolute peak (all nodes maxed) | Rare — used only for UPS sizing, not a real operating state |

**Realistic run cost (duty-cycled): roughly $24/month.** The 24/7 baseline accounts for most of that; the wake-on-demand nodes add relatively little since neither idles around the clock — one runs interactively, the other wakes on a nightly schedule.

Retiring the single largest legacy server was the biggest power lever in the fleet's history — a meaningful cut to the 24/7 baseline for zero loss of capability. Idle peripherals (monitors, unused displays) are put to sleep for the same reason: small, cheap wins add up when something runs 24/7.

## Roadmap

Software/configuration milestones are effectively free (time only); hardware milestones are itemized at a rough order of magnitude rather than exact figures, since prices move.

| Horizon | Milestone |
|---|---|
| **0–6 months** | Retire and harvest legacy hardware; bring up the inference workstation (GPU inference stack, dev tooling, offensive-security OS); bring up the services brain (chat/RAG front-end, vector DB, CI/CD, reverse proxy, VPN mesh); establish the security baseline (IDS/IPS, SIEM, secrets vault, CI scanners, isolated lab zone); build out the backup/bulk node |
| **6–12 months** | Larger-VRAM GPU upgrade to run bigger model classes on the inference host; RAM upgrade on the backup node for more cache headroom |
| **1–2 years** | NAS capacity refresh; platform upgrade for the inference workstation if demand grows; backup power (UPS) capacity increase |
| **2–5 years** | Dedicated next-generation inference server; faster core network backbone; storage refresh with a cold-archive tier |

The one hard architectural ceiling right now is GPU VRAM — it caps which model sizes can run locally at usable speed. The near-term upgrade path is a single higher-VRAM card rather than adding a second GPU, since the current platform only has one usable full-bandwidth slot. If concurrent usage grows past what a single-request inference server handles well, the next step is moving from a single-request-friendly inference engine to a serving engine built for concurrent requests.
