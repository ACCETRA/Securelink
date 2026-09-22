<p align="center">
  <img src="assets/securelink-mark.svg" width="112" alt="SecureLink shield and mesh icon" />
</p>

<h1 align="center">SecureLink</h1>

<p align="center">
  <strong>A decentralized trust layer for intermittently connected MeshCore networks.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/MeshCore-companion%20layer-38BDF8?style=for-the-badge" alt="MeshCore companion layer" />
  <img src="https://img.shields.io/badge/Trust-offline--capable-8B5CF6?style=for-the-badge" alt="Offline-capable trust" />
  <img src="https://img.shields.io/badge/Focus-security%20research-22C55E?style=for-the-badge" alt="Security research" />
</p>

---

## The idea

MeshCore already provides mesh communication and device cryptography. SecureLink does **not** alter its firmware or rebuild its cryptography. Instead, it adds a companion-layer trust system that helps people answer a practical question:

> Is this mesh node currently trusted—even when the Internet and blockchain are unavailable?

SecureLink maintains a minimal decentralized identity registry, distributes signed trust updates through bridges and the mesh, and lets nodes make clear, offline-aware verification decisions from their local trust cache.

## Trust at a glance

| Status | Meaning | User-facing signal |
| :---: | --- | --- |
| ✅ **Verified** | Registered and current trust information is available. | Safe to identify |
| ⚠️ **Stale** | Previously verified, but the cached state may be out of date. | Verify with caution |
| ❔ **Unknown** | No trusted registration is available locally. | Not yet verified |
| ❌ **Revoked** | The latest known registry state revokes the identity. | Do not trust |

## Architecture

```mermaid
flowchart TB
    chain[(Blockchain registry<br/>minimal identity state)]
    bridge[SecureLink bridge<br/>sync + signing]
    mesh{{MeshCore network<br/>radio transport + device crypto}}
    cache[(Local trust cache)]
    verify[Verification engine]
    ui[Clear trust UI<br/>✅ ⚠️ ❔ ❌]

    chain -->|identity, public key,<br/>version, status, timestamp| bridge
    bridge -->|signed trust updates| mesh
    mesh --> cache
    cache --> verify
    verify --> ui
```

### What belongs on-chain

| Stored | Never stored |
| --- | --- |
| Node identity and public key | Messages and chat content |
| Key version and lifecycle status | Radio traffic |
| Registration, rotation, revocation timestamps | Private keys |

## Research question

**How can decentralized identity verification improve trust management in intermittently connected MeshCore networks without modifying the underlying mesh firmware?**

SecureLink evaluates three dimensions:

- **Security:** impersonation, key substitution, revocation, altered trust data, replayed updates, and compromised identities.
- **Availability:** offline operation, stale caches, disconnected bridges, reconnection, and conflicting updates.
- **Usability:** explaining verified identity without requiring users to compare cryptographic keys.

## Proposed stack

<p>
  <img src="https://img.shields.io/badge/MeshCore-radio%20mesh-0F172A?style=flat-square&logo=bluetooth&logoColor=white" alt="MeshCore" />
  <img src="https://img.shields.io/badge/Node.js-bridge-339933?style=flat-square&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/Solidity-registry-363636?style=flat-square&logo=solidity&logoColor=white" alt="Solidity" />
  <img src="https://img.shields.io/badge/Hardhat-local%20chain-F7DF1E?style=flat-square" alt="Hardhat" />
  <img src="https://img.shields.io/badge/SQLite-trust%20cache-003B57?style=flat-square&logo=sqlite&logoColor=white" alt="SQLite" />
</p>

## Repository contents

| File | Purpose |
| --- | --- |
| `Decentralized Key Transparency Research.pdf` | Background research and evidence base. |
| `SecureLink_Decentralized_Key_Transparency.pptx` | Project presentation. |
| `build_hamza.py` / `build_pptx.py` | Presentation-generation sources. |
| `gen_assets.py` | Assets used by the presentation build. |

## Planned build path

1. Define the SecureLink identity and signed trust-update formats.
2. Build the minimal registration, key rotation, and revocation registry.
3. Implement a bridge that syncs registry state into an offline local cache.
4. Connect the bridge to MeshCore through its Companion Protocol.
5. Test trust decisions during offline, stale, replay, and revocation scenarios.
6. Present results through the four simple trust states above.

## Core principle

> **MeshCore is the ground layer. SecureLink is the decentralized trust layer above it.**

---

<p align="center">Built as a security and distributed-systems semester project.</p>
