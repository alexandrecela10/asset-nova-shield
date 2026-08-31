# Asset Nova: Shield

**An AI agent that reads every document on a megaproject, detects incoming contractor claims, finds weaknesses in the contractor's position, and drafts the owner's defense — before deadlines expire.**

This is a product case study for [Orbitron AI](https://orbitron.ai)'s Asset Nova product line. Shield is the first wedge: commercial claims defense for capital project owners.

## Live prototype

**[Open the prototype](https://alexandrecela10.github.io/asset-nova-shield/)** — a single-page interactive demo with three tabs:

| Tab | What's inside |
|-----|---------------|
| **Shield Demo** | Live walkthrough: agent detects a $42M differing site conditions claim, finds 3 weaknesses, drafts the owner's defense. Includes a collapsible daily risk dashboard design exploration. |
| **How This Was Built** | The PM process behind the case study — research, impact sizing, PRD, prototype, review panel. |
| **System Design** | Architecture, governance kernel, cascade pipeline. Collapsible agentic design exploration with eval tradeoffs. |

## The problem

| Stat | Source |
|------|--------|
| 89% of Middle East projects finish over budget | EY |
| 33% of capex ends in dispute on affected projects | HKA CRUX |
| $91M average dispute value in the Middle East | Arcadis |
| Most losses trace to missed notice deadlines and scattered evidence | Industry consensus |

These are reading problems, not judgment problems. AI solves them.

## Repository structure

```
index.html                          Live prototype (GitHub Pages)
aramco-pitch-deck.html              External pitch deck (owner audience)
orbitron-cto-pitch-deck.html        Internal pitch deck (CTO audience)
agent-walkthrough-deck.html         Step-by-step agent walkthrough
asset-nova-shield-deck.html         Product overview deck

artifacts/
  research/                         Discovery research, industry reports, user quotes
  analyses/                         Impact sizing, competitive analysis, context synthesis
  prds/                             Product PRD + AI engineering PRD
  roadmaps/                         Strategy, roadmap, delivery plan with OKRs
  validation/                       Quotes mapped to opportunities

synthetic-data/                     Fully synthetic project record for the demo
                                    (contract, geotech report, daily reports, correspondence)

commercial_guardian_agent/           Agent code + eval harness (Python, stdlib only)
eval-harness/                       Standalone eval harness with governance gates
```

## Agent design

Shield uses a **cascade pipeline** architecture optimized for recall first, then precision:

| Stage | Engine | Purpose |
|-------|--------|---------|
| Fast Triage | Cheap LLM / rules | Scan every document. Flag anything that might be a trigger event. Target: 100% recall. |
| Deep Analysis | Capable LLM | Evaluate flagged events against contract clauses. Add confidence scores. |
| Evidence Assembly | RAG + deterministic | Pull exact quotes, verify citations resolve to real documents. |
| Defense Draft | LLM + QA retry | Generate the owner's response. pass@k until citations verify. |
| Human Gate | Named approver | Nothing leaves the system without human approval. |

**Governance is the runtime, not a review step.** Every action passes an allowlist check. Out-of-scope documents are refused at ingest. "Send" is not a capability. Notices stop at `PENDING_APPROVAL` until a named human approves.

## Running the evals

```bash
cd commercial_guardian_agent
python3 run_evals.py          # human-readable report
python3 run_evals.py --json   # machine-readable
```

Zero external dependencies. Zero network calls. Exit code 0 only when all 17 evals pass. Governance evals are hard gates.

## Impact sizing

Per $5B megaproject, owner-side deployment:

| Scenario | Value protected | Multiple on $2M/yr license |
|----------|----------------|---------------------------|
| Worst case | $5-10M | 2-5x |
| Expected | ~$50M | ~25x |
| Best case | $100M+ | 50x+ |

Full methodology in [`artifacts/analyses/impact-sizing.md`](artifacts/analyses/impact-sizing.md).

---

Built by [Alexandre Cela](https://linkedin.com/in/alexandrecela) as a PM case study, using [PM OS](https://github.com/alexandrecela10/PM-OS) — an AI-native product management operating system.
