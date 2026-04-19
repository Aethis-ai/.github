# Aethis

**LLMs interpret rules. Aethis compiles them.**

AI agents are making eligibility and compliance decisions using LLM reasoning. Most of the time it works. When it doesn't, nobody notices — the model returns a confident, well-structured wrong answer with no audit trail.

We tested frontier LLMs on **225 hand-authored scenarios across four regulatory domains** with exception-chain depths up to 5:

| Model | spacecraft (n=68, depth 3) | construction (n=58, depth 5) | construction @ low reasoning (n=11) | Deterministic |
|--|:--:|:--:|:--:|:--:|
| **Aethis Engine** | **100%** | **100%** | **100%** | ✅ |
| GPT-5.4 | 100% | **96.6%** | **63.6%** | ❌ |
| Claude Sonnet 4.6 | 91.2% | — | — | ❌ |
| Claude Opus 4.6 | 89.7% | — | — | ❌ |
| GPT-5-mini | 75.0% | — | — | ❌ |
| GPT-4.1-mini | — | 79.3% | 45.5% | ❌ |

**No frontier model achieves 100% across all four paper domains.** GPT-5.4 — the strongest LLM tested — drops to 96.6% on construction insurance, and to 63.6% on the same domain when reasoning compute is reduced. Accuracy also varies with no change to prompts or inputs: re-runs of the same scenarios can produce different answers. In regulated workflows, decisions must be deterministic, explainable, and reproducible. LLMs fail all three regardless of peak accuracy.

### Key findings

- **Failures are systematic, not stochastic.** Under 10 independent runs on 7 failing spacecraft scenarios (70 trials), Claude Opus 4.6 produced **zero** correct answers (Clopper–Pearson 95% upper bound on per-trial success: 4.19%).
- **Prompt repair trades false negatives for false positives.** An enhanced prompt fixes 5 of 7 FN but introduces 20 FP, dropping net accuracy from 89.7% to 64.7% — Wilson CIs disjoint.
- **Failure pattern is exception-chain specific.** All frontier models achieve 100% on depth-2 English Language (43 scenarios). Failures emerge at depth 3 and deepen at depth 5.

**Paper:** [Confidently Wrong: Exception Chain Collapse in Frontier LLM Rule Evaluation](https://github.com/Aethis-ai/confidently-wrong-benchmark/blob/main/paper/Simpson_Exception_Chain_Collapse_2026.md) (Simpson, Kozak, Doake — April 2026)

[Full benchmark, paper, and reproduction scripts →](https://github.com/Aethis-ai/confidently-wrong-benchmark)

## How it works

An LLM compiles source legislation into formal logic once, at authoring time. After that, every decision is pure constraint evaluation — no LLM in the decision path. The engine finds the optimal question to ask next, short-circuits when a decision is reachable, and traces every answer back to the source clause.

## Repositories

| Repo | Description |
|------|-------------|
| [confidently-wrong-benchmark](https://github.com/Aethis-ai/confidently-wrong-benchmark) | Benchmark dataset, paper, and reproduction scripts — 225 paper-scope scenarios across 4 domains |
| [aethis-examples](https://github.com/Aethis-ai/aethis-examples) | Worked examples and test runner — run `uv run run_tests.py` with zero setup |
| [aethis-mcp](https://github.com/Aethis-ai/aethis-mcp) | MCP server — deterministic eligibility checks for any AI agent |
| [aethis-cli](https://github.com/Aethis-ai/aethis-cli) | Python CLI for the developer API |
| [aethis-sdk-python](https://github.com/Aethis-ai/aethis-sdk-python) | Official Python SDK for the developer API |

## Links

- [aethis.ai](https://aethis.ai) — Technology overview
- [aethis.legal](https://aethis.legal) — First application: UK immigration law
