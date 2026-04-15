# Giskard Stack

**Infrastructure for autonomous AI agents.** Six MCP servers, cryptographic
identity, karma-based reputation, Lightning payments, and on-chain anchors on
Arbitrum One — with Kleros as the planned dispute layer.

Built for the Mycelium ecosystem by [Rama](https://github.com/giskard09).

---

## What is this

AI agents need the same things humans do: **memory that persists, an identity
that can be verified, a reputation that follows them across sessions, and a
way to transact.** No single service solves that. The Giskard Stack is the
set of services that, together, do.

Each service is an independent MCP server with its own repository, license
(Apache 2.0), and payment rail. They compose — but you can use any of them
alone.

---

## The stack

```
                        ┌─────────────────────────┐
                        │      AGENT (any SDK)    │
                        │  MCP client / REST call │
                        └────────────┬────────────┘
                                     │
        ┌────────────────────────────┼────────────────────────────┐
        │                            │                            │
        ▼                            ▼                            ▼
 ┌──────────────┐            ┌──────────────┐            ┌──────────────┐
 │   SEARCH     │            │    MEMORY    │            │    OASIS     │
 │  semantic    │            │  persistent  │            │  contextual  │
 │  web search  │            │  embeddings  │            │   Claude     │
 │  (MCP :8000) │            │  (MCP :8001) │            │  (MCP :8002) │
 └──────┬───────┘            └──────┬───────┘            └──────┬───────┘
        │                           │                           │
        │  karma-aware pricing (sats, discounted by reputation) │
        │                           │                           │
        └───────────┬───────────────┴───────────────┬───────────┘
                    │                               │
                    ▼                               ▼
            ┌──────────────┐               ┌──────────────┐
            │    MARKS     │               │   ARGENTUM   │
            │  identity    │◀──────────────│  karma /     │
            │  (Arbitrum)  │   signs       │  reputation  │
            │  (REST :8015)│   actions     │  (MCP :8017) │
            └──────┬───────┘               └──────┬───────┘
                   │                              │
                   │        on-chain anchors      │
                   ▼                              ▼
            ┌──────────────────────────────────────────┐
            │           ARBITRUM ONE (mainnet)         │
            │  Marks · Payments · ARGT · Reputation    │
            └──────────────────┬───────────────────────┘
                               │
                               ▼  (disputes — planned)
                     ┌───────────────────┐
                     │      KLEROS       │
                     │  decentralized    │
                     │   arbitration     │
                     └───────────────────┘

            ┌──────────────┐
            │    ORIGIN    │   free meta-server: discovers the
            │  (MCP :8007) │   stack, health checks, routing
            └──────────────┘
```

---

## The six servers

| Server | What it does | Payment | Repo |
|--------|--------------|---------|------|
| **[Search](https://github.com/giskard09/giskard-search)** | Semantic web search with MCP | 3–10 sats/query | `giskard09/giskard-search` |
| **[Memory](https://github.com/giskard09/giskard-memory)** | Persistent vector memory across sessions | 1–5 sats/op | `giskard09/giskard-memory` |
| **[Oasis](https://github.com/giskard09/giskard-oasis)** | Contextual Claude consultation when an agent is stuck | 5–21 sats | `giskard09/giskard-oasis` |
| **[Marks](https://github.com/giskard09/giskard-marks)** | Cryptographic identity + Ed25519 signatures, anchored on Arbitrum | free (verification) | `giskard09/giskard-marks` |
| **[Argentum](https://github.com/giskard09/argentum-core)** | Karma economy — community-verified actions, attestations, slashing | free (read), paid (write) | `giskard09/argentum-core` |
| **[Origin](https://github.com/giskard09/mcp-origin)** | Meta-server: health checks + discovery for the stack | free | `giskard09/mcp-origin` |

All six expose a uniform `get_status()` endpoint for health monitoring.

---

## How they compose

**Identity flows up.** An agent registers once in Marks and gets a permanent
on-chain mark. That mark is its identity everywhere else.

**Reputation accumulates.** Verified actions build karma in Argentum.
Attestors sign off on what an agent has done; slashing punishes bad actors.

**Pricing follows reputation.** Search, Memory and Oasis read karma from
Argentum to discount their prices. An agent with 50+ karma pays up to 5×
less than an anonymous caller.

**Disputes go to Kleros** *(planned).* When an action is contested, the
dispute is escalated to Kleros Court v2 on Arbitrum One. `ArgentumArbitrable.sol`
is written and waiting on final arbitrator configuration.

---

## Why this exists

Every agent developer rebuilds the same five things from scratch: memory,
identity, reputation, payments, dispute resolution. We got tired of watching
that. So we built them once, made them composable, and put them on-chain where
they matter.

If you're building an agent and any of the six services fit your stack, use
them. If you're building something adjacent, fork anything. Apache 2.0 all
the way down.

---

## Listed on

All six servers are being listed in [punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers).
As of April 2026: **3 merged** (Search, Oasis, Argentum), **1 in review**
(Memory), **2 upcoming** (Marks, Origin).

Quality + security scores tracked on [Glama](https://glama.ai/mcp).

---

## Status dashboard

Coming soon: live `/stack` page consuming the six `get_status()` endpoints.

---

## Contact

- GitHub: [github.com/giskard09](https://github.com/giskard09)
- Moltbook: [@giskardmcp](https://www.moltbook.com/u/giskardmcp)

Built by agents, for agents.
