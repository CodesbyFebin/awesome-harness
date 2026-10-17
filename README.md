# Awesome Harness — Master Merge of the ruvnet Agent Ecosystem

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:010409,100:00ff00&height=200&section=header&text=Awesome%20Harness&fontSize=48&fontAlignY=35&desc=The%20complete%20open-source%20agent%20harness%20stack&descAlignY=60&descSize=18" alt="Awesome Harness Hero Banner" width="100%" />
</p>

<p align="center">
  <a href="https://github.com/ruvnet/ruflo"><img src="https://img.shields.io/badge/Ruflo-76k%20stars-00ff00?style=for-the-badge&logo=github" alt="Ruflo" /></a>
  <a href="https://github.com/ruvnet/metaharness"><img src="https://img.shields.io/badge/MetaHarness-factory-00ff00?style=for-the-badge" alt="MetaHarness" /></a>
  <a href="https://github.com/ruvnet/RuVector"><img src="https://img.shields.io/badge/RuVector-memory-00ff00?style=for-the-badge" alt="RuVector" /></a>
  <a href="DARWIN.md"><img src="https://img.shields.io/badge/Darwin%20Mode-evolve-00ff00?style=for-the-badge" alt="Darwin" /></a>
</p>

> **The complete open-source agent harness stack.**
>
> Curated master catalog and integration guide for Reuven Cohen (rUv / ruvnet) / Cognitum One's essential repositories that form the modern agent harness ecosystem.
>
> Built as a living reference for systems engineers who want to deploy, extend, and govern multi-agent systems with Claude Code, Codex, Hermes, and beyond.
>
> Aesthetic: near-black evidence-console / phosphor-green terminal. Systems that prove what happened.

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

This repository aggregates the essential pieces into one reference so you can understand, install, and compose them. It is not a monorepo of the original code; it is the map and the integration manual.

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

## Darwin Mode — Freeze the Model, Evolve the Harness

**Darwin Mode** (`@metaharness/darwin`) is the self-improvement engine of the MetaHarness stack.

> Keep your model frozen. Evolve the operating system around it (planners, context builders, reviewers, routing policies, verification steps, memory strategies, repair loops) so a cheap model performs like an expensive one.

**Full reference:** [DARWIN.md](DARWIN.md) (mutation surfaces, scoring gate, safety model, CLI flags, GEPA, evolutionary stack, API).

### Core Idea
- Model weights stay fixed.
- The harness configuration is mutated, tested in a sandbox, and only variants that measurably improve cost / accuracy / latency / safety are promoted.
- Winners become part of an evolutionary archive. Improvements compound.
- Safe by default: no network, no API key required for pure refactor/tuning paths; safety inspector + promotion gate.

### Key Measured Results (SWE-bench Lite, conformant)
- Open-loop cheap model: ~7.7%
- After successive harness evolution + repair + routing + multi-tier: **58.3%** (same model, ~7.6× improvement)
- Single interactive trajectory (DeepSeek-V4-Flash): **34.0%** at ~$0.005/instance
- Test-Driven Repair (given the acceptance test): **68.3%** at ~$0.01–0.08/instance
- Best-of-3 + LLM-judge selection: ~52% (pilot)

Sits on the cost-Pareto frontier versus systems that spend $0.1–$2.5+ per instance.

### Two Main Modes (ADR-175)
| Mode | When | What you get |
|------|------|--------------|
| **Test-Driven Repair** (default) | You have a failing CI test | Verified-fix PR (CI Autofixer) |
| **Conformant** (`--no-test-oracle`) | Ticket only, no test | Agent writes its own `reproduce_bug.py` + MCTS search |

### How to Use
```bash
# In a MetaHarness-generated project
npm run evolve

# Or directly
npx @metaharness/darwin
npm install @metaharness/darwin

# CLI evolve verb
metaharness-darwin evolve <repo> [--generations N] [--children N] [--selection score|quality-diversity|...]
```

Every scaffold from `npx metaharness` ships with Darwin Mode wired in by default (`--no-darwin` to skip).

### Related Extensions
- **Darwin Shield** (ADR-155): same thesis applied to defensive vulnerability discovery (Semgrep + property fuzzer oracles).
- **Weight-EFT**: complementary lever — distills gold-resolved archives into LoRA tunes on the cheap tier so escalations to frontier models happen less often. The tune itself is a gene Darwin can prune.
- **GEPA**: learning harness that evolves policy genomes against scored task corpora (holdout promotion only).
- Integrated into Ruflo as an optional, removable augmentation (ADR-153).

Lineage: Darwin Gödel Machine style — mutate the agent source, empirically validate each variant.

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

### 5. Evolve the Harness
```bash
npm run evolve   # inside a MetaHarness scaffold
```

### 6. Full Local Clone of Essentials
```bash
git clone https://github.com/ruvnet/ruflo.git
git clone https://github.com/ruvnet/metaharness.git
git clone https://github.com/ruvnet/RuVector.git
git clone https://github.com/ruvnet/rvm.git
```

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
   +------+------+                       +------|------+
          |                                     |
          v                                     v
   +------+------+     +----------+     +------v------+
   | RuVector    | <--> | Memory   | <--> | Darwin Mode|
   | (Vector+GNN)|     | Layers   |     |  (Evolve)  |
   +------+------+     +----------+     +-------------+
          |
          v
   +------+------+     +----------+
   |   RVM /     |     | Sensing  |
   | Sandboxes   |     | (RuView) |
   +-------------+
```

### Composition Patterns

1. **Start with Ruflo** for an immediate multi-agent swarm with memory and federation.
2. **Generate a focused harness** with MetaHarness when you want your own branding and reduced surface.
3. **Plug RuVector** for shared, self-improving memory across agents.
4. **Evolve with Darwin** so the harness itself improves on your workload.
5. **Isolate with RVM** when untrusted code or peer agents must run under hardware capabilities.
6. **Govern with ruClip** when you need org-level budget, approval, and audit.

---

## Integration Notes

- **ADR-150** in Ruflo makes MetaHarness an optional, removable augmentation. Ruflo works without it.
- **ADR-153** covers Darwin Mode integration into Ruflo (optional, graceful degradation).
- Generated harnesses from MetaHarness are fully owned by you (your branding, your `npx` CLI, witness-signed).
- Memory layers (RuVector / agentdb) are designed to be shared across agents and improve with usage.
- Evolution (Darwin Mode, Dream Machine, Autogenous) is evidence-gated and reversible.
- Hosts supported by MetaHarness: Claude Code, Codex, pi.dev, Hermes, OpenClaw, RVM, GitHub Copilot, OpenCode, Prime Agent, GitHub Actions.
- Safety is first-class: default-deny MCP, scrubbed sandboxes, witness-signed releases, capability tokens in RVM.

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
19. ruvnet/rudevolution
20. ruvnet/agenticow

(And ~200 more public repos covering quantum, neural, sensing, decompilers, etc.)

---

## Suggested Topics for This Repo

```
agent-harness
meta-harness
ruflo
metaharness
ruvector
rvm
darwin-mode
agent-swarm
mcp
claude-code
codex
multi-agent
self-improving
open-source
```

---

## Contributing / Updating This Catalog

This is a living master reference. PRs welcome for:

- Updated star counts and status
- New essential repos
- Better integration recipes
- Architecture diagrams
- Submodule or monorepo experiments
- Hero banner refinements

---

## Credits

- All original work by **Reuven Cohen (rUv)** / [ruvnet](https://github.com/ruvnet) / Cognitum One
- This master catalog assembled as a reference for systems engineers building on the open agent harness stack
- Brand aesthetic inspired by evidence-console / phosphor-green terminal style

---

**Long live open source.**

Built by CodesbyFebin — systems that prove what happened.

---

*Suggested repository description:*  
Master catalog and integration guide for the ruvnet / Cognitum One open-source agent harness ecosystem (Ruflo, MetaHarness, RuVector, Darwin Mode, RVM, ruClip).
