# Awesome Harness — Master Merge of the ruvnet Agent Ecosystem

> **The complete open-source agent harness stack.**
>
> Curated master catalog and integration guide for Reuven Cohen (rUv / ruvnet) / Cognitum One's essential repositories that form the modern agent harness ecosystem.
>
> Built as a living reference for systems engineers who want to deploy, extend, and govern multi-agent systems with Claude Code, Codex, Hermes, and beyond.

---

## What is a Harness?

An **agent** is a model plus a **harness**.

- The model reasons and writes.
- The harness supplies tools, memory, workflows, sandboxes, governance, and the control plane that lets it do real work.

The ruvnet stack is the most mature open-source realization of this idea, with:

- **Ruflo** as the original agent meta-harness
- **MetaHarness** as the factory that generates custom harnesses
- **RuVector** as the self-learning vector + GNN memory layer
- **RVM** as the agentic virtual machine / sandbox
- Supporting control planes, evolution engines, and domain verticals

This repository aggregates the essential pieces into one reference so you can understand, install, and compose them.

---

## Flagship Systems (Core Stack)

| Project | Stars | Role | Link |
|---------|-------|------|------|
| **ruflo** | ~76k | Original agent meta-harness. Multi-player swarms, adaptive memory, self-learning, federation, MCP, Claude Code / Codex native. | [ruvnet/ruflo](https://github.com/ruvnet/ruflo) |
| **metaharness** | ~700 | Meta-harness factory. Scaffold your own branded harness (CLI, MCP, memory, Darwin evolution, witness-signed releases). Works with 10+ hosts. | [ruvnet/metaharness](https://github.com/ruvnet/metaharness) |
| **RuVector** | ~4.6k | High-performance real-time self-learning vector + GNN database in Rust. Agent memory that improves with use. | [ruvnet/RuVector](https://github.com/ruvnet/RuVector) |
| **RuView** | ~98k | Privacy-first WiFi spatial intelligence (presence, vitals, pose) — the sensing substrate that can feed agent systems. | [ruvnet/RuView](https://github.com/ruvnet/RuView) |
| **rvm** | ~150 | Virtual Machine built for the Agentic Age (Rust). Hypervisor, capabilities, bare-metal, no-std, witness. Hardware isolation. | [ruvnet/rvm](https://github.com/ruvnet/rvm) |
| **ruClip** | ~20 | Control plane for running an AI-agent company: org-chart, budget-gated heartbeats, approval gates, signed audit trail. Built on ruflo + metaharness + RuVector. | [ruvnet/ruClip](https://github.com/ruvnet/ruClip) |

---

## Essential Supporting Repositories

### Memory & Vector
- **agentdb** — Vector memory that gets smarter every time an agent uses it.
- **RuLake** — Cache-coherent vector execution fabric that sits in front of any vector store.
- **agenticow** — Git for Agent Memory (copy-on-write vector branching).

### Evolution & Governance
- **dream-machine** — Config-driven engine for nightly, cloud-scheduled, evidence-gated repository evolution (uses MetaHarness flywheel/Darwin).
- **autogenous** — Governed evolutionary software: runtime failures → verified, portable, reversible adaptations.
- **APx** — Agentic Power framework: measures useful work by task, quality, cost, safety, and human effort.

### Runtimes & Discovery
- **Agent-Name-Service** — Secure discovery and interaction protocol for AI agents (OWASP GenAI).
- **AgentBBS** — Multiplayer AI community (humans + agents over SSH/MCP, boards, marketplace, arena).
- **rGi** — Perpetual agentic runtime for AGI built on the ruvstack.
- **openAVO** — Open harness for long-horizon autonomous reasoning (prediction, memory, verification, adaptive action).

### Model Routing & Cost
- **agentic-flow** — Switch between low-cost models in Claude Code / Agent SDK; deploy hosted agents.
- MetaHarness `@metaharness/router` — Learned cost-optimal model routing.

### Domain & Specialized
- **sparc** — SPARC methodology tooling.
- **flow-nexus** — Competitive agentic platform on MCP.
- **marketing** — Agentic marketing swarm.
- Many vertical templates inside MetaHarness (`vertical:coding`, `vertical:research`, etc.).

---

## Quick Start Paths

### 1. Use the Original Harness (Ruflo)
```bash
# Claude Code
/plugin marketplace add ruvnet/ruflo
# then install ruflo-console and ruflo-mods

# Or general
npx ruflo@latest init
npx -y ruflo@latest mcp start
```

### 2. Generate Your Own Branded Harness (MetaHarness)
```bash
npx metaharness my-agent --template vertical:coding --host claude-code
cd my-agent && npx . --help

# Or analyze an existing repo
npx metaharness analyze-repo . --scaffold my-bot
```

### 3. Add Self-Learning Memory
```bash
# RuVector (Rust core + bindings)
cargo add ruvector   # or npm / python equivalents
```

### 4. Run Under Hardware Isolation
Use RVM for capability-based, bare-metal agent execution.

---

## Architecture Overview

```
                    +------------------+
                    |   Control Plane  |
                    |     (ruClip)     |
                    +--------+---------+
                             |
          +------------------+------------------+
          |                                     |
   +------v------+                       +------v------+
   |   Ruflo     |                       | MetaHarness |
   |  (Harness)  | <--- generates --->   |  (Factory)  |
   +------+------+                       +-------------+
          |
          v
   +------+------+     +----------+     +----------+
   | RuVector    | <--> | Memory   | <--> | Agents   |
   | (Vector+GNN)|     | Layers   |     | (Swarms) |
   +------+------+     +----------+     +----------+
          |
          v
   +------+------+     +----------+
   |   RVM /     |     | Sensing  |
   | Sandboxes   |     | (RuView) |
   +-------------+
```

---

## Integration Notes

- **ADR-150** in Ruflo makes MetaHarness an optional, removable augmentation. Ruflo works without it.
- Generated harnesses from MetaHarness are fully owned by you (your branding, your `npx` CLI, witness-signed).
- Memory layers (RuVector / agentdb) are designed to be shared across agents and improve with usage.
- Evolution (Darwin Mode, Dream Machine, Autogenous) is evidence-gated and reversible.
- Hosts supported by MetaHarness: Claude Code, Codex, pi.dev, Hermes, OpenClaw, RVM, GitHub Copilot, OpenCode, Prime Agent, GitHub Actions.

---

## Full Catalog (High-Signal Repos)

Sorted roughly by relevance to the harness stack:

1. ruvnet/ruflo
2. ruvnet/metaharness
3. ruvnet/RuVector
4. ruvnet/rvm
5. ruvnet/ruClip
6. ruvnet/agentdb
7. ruvnet/dream-machine
8. ruvnet/agentic-flow
9. ruvnet/Agent-Name-Service
10. ruvnet/AgentBBS
11. ruvnet/RuView
12. ruvnet/autogenous
13. ruvnet/APx
14. ruvnet/openAVO
15. ruvnet/rGi
16. ruvnet/RuLake
17. ruvnet/sparc
18. ruvnet/flow-nexus

(And ~200 more public repos covering quantum, neural, sensing, decompilers, etc.)

---

## Contributing / Updating This Catalog

This is a living master reference. PRs welcome for:

- Updated star counts and status
- New essential repos
- Better integration recipes
- Architecture diagrams
- Submodule or monorepo experiments

---

## Credits

- All original work by **Reuven Cohen (rUv)** / [ruvnet](https://github.com/ruvnet) / Cognitum One
- This master catalog assembled as a reference for systems engineers building on the open agent harness stack
- Brand aesthetic inspired by evidence-console / phosphor-green terminal style

---

**Long live open source.**

Built by CodesbyFebin — systems that prove what happened.
