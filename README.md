# Giskard Stack

**The accountability layer for any entity that acts.**

Six MCP servers, cryptographic identity, karma-based reputation, Lightning
payments, post-execution trails, and on-chain anchors on Arbitrum One + Base —
with Kleros as the dispute layer.

Part of the [RAMA Weave](https://github.com/giskard09/rama-core). Built by [Rama](https://github.com/giskard09).

---

## What is this

Autonomous agents — and the humans who deploy them — need the same things:
**memory that persists, an identity that can be verified, a reputation that
follows them across sessions, a way to transact, and proof of what actually
happened after the fact.**

No single service solves that. The Giskard Stack is the set of services that,
together, do.

Each service is an independent MCP server with its own repository, license
(Apache 2.0), and payment rail. They compose — but you can use any of them
alone.

---

## The stack

```
                        ┌─────────────────────────┐
                        │    ENTITY (any SDK)     │
                        │  agent · human · robot  │
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
                   ▼                              ▼
            ┌──────────────────────────────────────────┐
            │        MYCELIUM TRAILS                   │
            │  post-execution accountability receipts  │
            │  payment_hash · action_ref · on-chain    │
            │  anchor · Arbitrum One + Base mainnet    │
            └──────────────────┬───────────────────────┘
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
           ARBITRUM ONE             BASE MAINNET
           (identity · karma)   (trails · payments)
                    │
                    ▼  (disputes)
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

## The seven servers

| Server | What it does | Payment | Repo |
|--------|--------------|---------|------|
| **[Search](https://github.com/giskard09/giskard-search)** | Semantic web search | 3–10 sats/query | `giskard09/giskard-search` |
| **[Memory](https://github.com/giskard09/giskard-memory)** | Persistent vector memory across sessions | 1–5 sats/op | `giskard09/giskard-memory` |
| **[Oasis](https://github.com/giskard09/giskard-oasis)** | Contextual Claude consultation when an agent is stuck | 5–21 sats | `giskard09/giskard-oasis` |
| **[Marks](https://github.com/giskard09/giskard-marks)** | Cryptographic identity + Ed25519 signatures, anchored on Arbitrum | free | `giskard09/giskard-marks` |
| **[Argentum](https://github.com/giskard09/argentum-core)** | Karma economy — community-verified actions, attestations, slashing | free (read) | `giskard09/argentum-core` |
| **[Mycelium Trails](https://github.com/giskard09/giskard-stack)** | Post-execution accountability receipts — signed, payment-linked, dual-chain anchored | 21 sats/trail | `giskard09/giskard-stack` |
| **[Origin](https://github.com/giskard09/mcp-origin)** | Meta-server: health checks + discovery | free | `giskard09/mcp-origin` |

---

## How they compose

**Identity flows up.** An entity registers once in Marks and gets a permanent
on-chain mark. That mark is its identity everywhere else.

**Reputation accumulates.** Verified actions build karma in Argentum.
Attestors sign off on what an entity has done; slashing punishes bad actors.

**Pricing follows reputation.** Search, Memory and Oasis read karma from
Argentum to discount their prices. An entity with 50+ karma pays up to 5×
less than an anonymous caller.

**Execution leaves proof.** Mycelium Trails records what happened after payment
settled — a signed, on-chain-anchored receipt linking payment_hash to action.
Verifiable by anyone. Usable in disputes, audits, insurance, and reputation scoring.

**Disputes go to Kleros.** When an action is contested, the trail is the
evidence. `ArgentumArbitrable.sol` is written and waiting on final configuration.

---

## Ecosystem

Mycelium Trails is part of the **Agent Trust Loop** — a verifiable accountability
stack built with:

- [Agent Passport System](https://github.com/aeoess/agent-passport-system) — identity + governance
- [Sentinel Alpha](https://github.com/chox-cell/Sentinel-Alpha) — law-bound AI infrastructure
- [x402](https://github.com/x402-foundation/x402) — agent-native payments
- [AgentKit](https://github.com/coinbase/agentkit/pull/1170) — Coinbase agent framework (PR in review)

The coordination layer: [RAMA](https://github.com/giskard09/rama-core) — a weave of entities
aligned around verifiable accountability.

---

## Why this exists

Every agent developer rebuilds the same things from scratch: memory,
identity, reputation, payments, proof of execution. We got tired of watching
that. So we built them once, made them composable, and put them on-chain where
they matter.

If you're building an agent and any of the seven services fit your stack, use
them. If you're building something adjacent, fork anything. Apache 2.0 all
the way down.

---

## Listed on

**[Official MCP Registry](https://registry.modelcontextprotocol.io)** — five
servers live under the `io.github.giskard09/*` namespace, all `active`:

- `io.github.giskard09/argentum` · v0.4.0
- `io.github.giskard09/memory` · v1.0.1
- `io.github.giskard09/oasis` · v0.1.0
- `io.github.giskard09/origin` · v0.1.0
- `io.github.giskard09/search` · v0.1.1

Also indexed on [punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)
and [Glama](https://glama.ai/mcp).

---

## Contact

- GitHub: [github.com/giskard09](https://github.com/giskard09)
- Moltbook: [@giskardmcp](https://www.moltbook.com/u/giskardmcp)

Built by humans and agents, for any entity that acts.
