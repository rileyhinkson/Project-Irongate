# Architecture

## Executive summary

IronGate is a fully self-hosted environment in which local AI models power a private software-development workflow and a defensive security operation, backed by an isolated offensive-security lab for hands-on skills work. Nothing leaves the network by default; every service is reachable remotely over an encrypted mesh VPN rather than any inbound port-forward.

The fleet is consolidated around a small number of purpose-built nodes: a durable NAS, an always-on services brain, a GPU inference and development host, and a wake-on-demand backup + auxiliary-AI node. Legacy machines are retired and harvested for parts rather than left running. The result runs at a low, mostly-idle power draw, scales capability without materially growing the 24/7 baseline, and doubles as a living portfolio for a Security+ / CySA+ / Linux+ certification trajectory.

## Design principles

- **Sovereignty / local-first** — models, code, secrets, and logs stay on-network. Cloud services are the exception, not the default.
- **Separation of concerns** — hot compute, warm bulk storage, and durable NAS live in distinct chassis so one failure domain can't cascade into another.
- **Segmentation** — the network is split into purpose-built zones, with the security lab fully isolated from production (no egress, no route back in).
- **Right tool per watt** — always-on load runs on efficient silicon; heavy iron wakes on demand instead of idling 24/7.
- **Data authority + backup** — one authoritative storage tier, one independent replica, following a 3-2-1 backup pattern.

## Fleet roster

Each node has a single clear job. Names below are project codenames, not real hostnames.

| Node | Role | Power profile |
|---|---|---|
| **Foundry** | Primary NAS — storage of record | 24/7 |
| **Bellows** | Services brain — AI front-end, CI/CD, SIEM | 24/7 |
| **Anvil** | LLM inference · remote dev workstation · security lab host | Wake-on-demand |
| **Kiln** | Bulk storage / backup replica · auxiliary AI (embeddings, transcription) | Wake-on-demand |
| **Gate** | Router / firewall / IDS | 24/7 |
| **Warden** | DNS ad-blocking · VPN mesh exit node | 24/7 |
| **Core** | L2 switch — VLAN segmentation | 24/7 |
| **AP** | Wireless access point | 24/7 |
| **Hearth** | Media / gaming, kept isolated from the lab | On-demand |

Client devices (laptops, phones) ride the wireless network as trusted clients rather than rack nodes.

## Node roles

**Foundry — Primary NAS.** Storage of record for the live retrieval corpus, datasets, and snapshots, served over the internal LAN. Runs on ECC memory over an established RAID pool — every other node treats this as the authoritative copy of the data.

**Bellows — Services brain.** The efficient, always-on control plane: a private chat + retrieval front-end for the local LLMs, the vector database, a self-hosted git server with CI/CD, a SIEM, a self-hosted password/secrets vault, reverse proxy, and the VPN mesh's subnet router. Sized to carry all of it at a low idle wattage.

**Anvil — AI inference, dev & security workstation.** The workstation doubling as the inference host. Runs a Linux distribution tuned for GPU inference (native ROCm support, no workaround hacks needed), with a second, fully separate OS install dedicated to offensive-security tooling. Models run split-role: a larger model for chat/refactor work, a small model for autocomplete. Reached over remote-SSH via the VPN mesh — never exposed directly.

**Kiln — Bulk storage / backup + auxiliary AI.** Harvested hardware repurposed into a self-hosting node: a large parity-protected pool serving as the nightly replication target for Foundry, plus a spare GPU running transcription and embedding jobs against the local knowledge corpus. Sleeps by default; wakes on a schedule for replication and batch jobs, then goes back to sleep.

## Retired / harvested

Older hardware is retired deliberately rather than left running for sentimental reasons — each retirement is evaluated for its power-draw impact first. Components (drives, RAM, PSUs, GPUs) are harvested into active nodes where useful; the rest is sold or recycled. The single biggest power-saving decision in the fleet's history was retiring one large, power-hungry server, cutting the 24/7 baseline meaningfully with zero loss of capability.

## Storage strategy

- **Foundry** is authoritative: live corpus, datasets, home directories.
- **Kiln** is an independent replica + bulk store, fed by scheduled, incremental replication from Foundry.
- **Anvil** keeps hot models on local fast storage for quick load; cold copies sync back to the bulk tier.

Net effect: a 3-2-1-style backup pattern across two hosts and two storage media types — one authoritative copy, one independent replica, with regular integrity scrubs on both pools.
