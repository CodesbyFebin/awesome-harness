# Darwin Mode — Full Reference

**Source:** [@metaharness/darwin](https://www.npmjs.com/package/@metaharness/darwin) · [ruvnet/metaharness/packages/darwin-mode](https://github.com/ruvnet/metaharness/tree/main/packages/darwin-mode)

> **An LLM supercharger and cost optimizer.** Keep your model frozen — evolve the harness around it so a *cheap* model performs like an expensive one, for a fraction of the cost.

Darwin Mode makes the LLM you already use **measurably better and cheaper** by evolving the *operating system around it* — planner, context builder, reviewer, retry/tool/memory/score policy — instead of paying for a bigger model. It mutates one surface at a time, tests each change in a sandbox, and keeps only what *measurably* improves, building an archive of successful descendants. No weight updates, no fine-tuning — just a population, a benchmark, and an archive.

This follows the **Darwin Gödel Machine** lineage: iteratively mutate the source of a coding agent, then *empirically validate* each variant.

---

## Why It Pays Off (Measured)

- **Conformant bug-fixing for half a cent.** Single interactive trajectory (DeepSeek-V4-Flash, no gold tests in-loop) resolves **34.0%** of real **SWE-bench Lite** issues — `102/300`, Wilson 95% CI `[28.9, 39.5]`, **~$0.005/instance** — full 300-instance set, official `swebench` Docker harness.
- **Best-of-3 + LLM-judge selection ≈ 52%** (pilot, n=25, conformant) at **~$0.015/instance**.
- **Cost-Pareto frontier.** At ~$0.005–0.015/instance Darwin sits on the resolve-per-dollar frontier versus systems that spend $0.1–$2.5+ per instance. Live leaderboard: [cost-pareto.html](https://ruvnet.github.io/agent-harness-generator/cost-pareto.html).
- **The harness is the multiplier.** Breakthrough was a scaffold change (MCTS → stateful interactive ReAct with the repo's own tests as the regression gate), same cheap model.

**Historical progression (same model):** open-loop ~7.7% → successive generations of harness evolution + repair + routing + multi-tier → **58.3%** (~7.6×).

---

## ⭐ The Product: Test-Driven Repair (TDR)

**Hand Darwin a failing test, get a verified-fix PR — at ~$0.01–0.08/instance.**

On SWE-bench Lite, TDR resolves **68.3%** of real issues *when given the acceptance test* (the realistic CI/CD setting). This is a with-acceptance-test number, **not** a leaderboard claim.

### Two Modes (ADR-175)

| Mode | When | Signal | What you get |
|------|------|--------|--------------|
| **Test-Driven Repair** ⭐ (default) | You have a failing/CI test | Gate on **your** test | **CI Autofixer** — verified-fix PR for pennies, **68.3%** with-test |
| **Conformant** (`--no-test-oracle`) | No test, just a ticket | Agent writes its own `reproduce_bug.py` + MCTS search | **Legacy Modernizer** — best-effort fix when no test exists |

Same engine, one flag. TDR is the product that matters for production CI.

---

## Evolution Loop

```
repo
  → profile      RepoProfile (pkg mgr, test cmd, source/risk files)
  → baseline     generate the seven mutation-surface files
  → mutate       pick ONE approved surface, perturb it (behind the gate)
  → sandbox      safety-inspect → run the test command (no shell, no net, no secrets)
  → score        weighted base score − hard penalty layer
  → archive      record parent→child as a TREE (not a single best branch)
  → select       sample the next generation from the WHOLE archive
  → repeat
```

Dependency-free: **Node ≥ 20 built-ins only**, no runtime dependencies.

---

## The Seven Mutation Surfaces

A child mutates **exactly one** surface per generation. Only these seven files are allowed (enforced by safety inspector):

| Surface | File | Governs |
|---------|------|---------|
| `planner` | `planner.ts` | task string → ordered plan steps |
| `contextBuilder` | `context_builder.ts` | ranks candidate files by term overlap |
| `reviewer` | `reviewer.ts` | flags changed files against risk list + test outcome |
| `retryPolicy` | `retry_policy.ts` | whether/how to retry on symbolic failure |
| `toolPolicy` | `tool_policy.ts` | allow-list + deterministic ordering |
| `memoryPolicy` | `memory_policy.ts` | whether an outcome is worth remembering |
| `scorePolicy` | `score_policy.ts` | weight vector proposal (kernel scorer decides promotion) |

---

## Scoring & Promotion Gate

Pure function scorer (ADR-072). Re-running on the same traces yields identical verdict.

```
baseScore = 0.35·taskSuccess + 0.20·testPassRate + 0.15·traceQuality
          + 0.10·costEfficiency + 0.10·latencyEfficiency + 0.10·safetyScore

finalScore = baseScore − 0.30·secretExposure − 0.25·destructiveAction
                       − 0.20·hallucinatedFile − 0.15·toolLoop − 0.10·costOverrun
```

A child replaces its parent only when **all four** hold:

1. `finalScore > parent + promotionDelta` (default 0.05)
2. `safetyScore ≥ 0.95`
3. `testPassRate ≥ parentTestPassRate`
4. `safetyScore == 1.0` (zero blocked actions)

Non-promoted variants are retained (tree, not single best branch). Selection samples the whole archive so evolution can escape local optima.

---

## Safety Model (Load-Bearing)

Two independent checks before anything runs:

- `inspectVariant(dir)` — before execution. Rejects anything outside the seven files, blocked filenames, symlinks, nested dirs, or forbidden content (`process.env`, `child_process`, `eval`, `fetch`, shell strings, etc.).
- `validateGeneratedCode(code)` — before writing LLM-generated code. Violations are discarded.

Sandbox is **shell-free** (`execFile`, never a shell) and runs under a **scrubbed environment** (only PATH + 3 identifying vars). Secrets never reach a variant. Disqualified variants get exit code 99 and cannot promote.

---

## Quick Start & CLI

```bash
npm install @metaharness/darwin
npx @metaharness/darwin
# or inside a MetaHarness scaffold:
npm run evolve
```

Full CLI:

```bash
metaharness-darwin evolve <repo> [--generations N] [--children N] [--concurrency N] [--seed N] \
    [--bench <suite.json>] [--tie faster] \
    [--selection score|quality-diversity|behavioral-diversity|niche-steering|clade|pareto] \
    [--crossover] [--epistasis] [--risk-budget N] [--fdr Q] [--curriculum] \
    [--sandbox real|mock|agent]
```

Defaults: generations=3, children=4, concurrency=4, seed=0, sandbox=real.

Artifacts land in `<repo>/.metaharness/`:
- `archive.json` — population tree
- `lineage.json`
- `variants/` — one dir per variant
- `runs/` — traces + scores
- `reports/winner.json`

---

## Programmatic API

```ts
import { evolve } from '@metaharness/darwin';

const result = await evolve({
  repoRoot: '/abs/path/to/repo',
  workRoot: '/abs/path/to/repo/.metaharness',
  generations: 3,
  childrenPerGeneration: 4,
  concurrency: 4,
  promotionDelta: 0.05,
  seed: 0,
  tasks: ['run repository test suite', 'verify generated harness safety'],
});

result.winner;        // best ArchiveRecord
result.winnerLineage; // ['baseline', 'g1_v0', 'g2_v1']
result.records;       // full tree in insertion order
```

---

## GEPA — Prompt-Policy Learning

`@metaharness/darwin/gepa` applies the same thesis to prompt policy genomes.

- Genome of named text components.
- Reflection LM proposes single-component mutations from textual feedback.
- Pareto selection over per-instance scores.
- Strict holdout promotion: gold-no-regress AND empty-patch-improves AND cost/resolved-not-worse.

First promoted genome: **cand-6** (holdout gold 2/12 → 3/12, zero regressions).

You supply the evaluator; the engine and promotion rule ship in the package.

---

## Evolutionary Stack (Opt-In Additions)

All off by default so the core stays byte-identical and reproducible:

| Capability | Flag / Config |
|------------|---------------|
| Efficiency tie-break | `--tie faster` |
| Statistical benchmark gate | `--bench suite.json` |
| MAP-Elites / quality diversity | `--selection quality-diversity` |
| Genetic crossover | `--crossover` |
| Epistatic linkage | `--crossover --epistasis` |
| Risk budget (SGM) | `--risk-budget N` |
| FDR control | `--fdr Q` |
| Curriculum | `--curriculum` |
| Pareto multi-objective | `--selection pareto` |
| Behavioral diversity / niche steering | `--selection behavioral-diversity` / `niche-steering` |
| Clade metaproductivity | `--selection clade` |
| LLM mutator (OpenRouter) | `config.generator` |

Sandbox modes: `real` (repo tests), `mock` (surface-driven loop), `agent` (real surface code).

---

## Related

- **Darwin Shield** (ADR-155): same engine applied to defensive vulnerability discovery (Semgrep + property fuzzer oracles).
- **Weight-EFT**: complementary LoRA distillation of gold-resolved archives onto the cheap tier.
- Integrated into Ruflo as optional removable augmentation (ADR-153).
- Every MetaHarness scaffold ships with `npm run evolve` wired by default (`--no-darwin` to disable).

---

**Long live open source.**

Extracted and organized for the [awesome-harness](https://github.com/CodesbyFebin/awesome-harness) master catalog.
