# Aethis

**LLMs interpret rules. Aethis compiles them.**

AI agents are making eligibility and compliance decisions using LLM reasoning. Most of the time it works. When it doesn't, nobody notices — the model returns a confident, well-structured wrong answer with no audit trail. In regulated workflows, decisions must be deterministic, explainable, and reproducible. LLMs fail all three regardless of peak accuracy.

## How it works

An LLM compiles source legislation into formal logic once, at authoring time. After that, every decision is pure constraint evaluation — no LLM in the decision path. The engine finds the optimal question to ask next, short-circuits when a decision is reachable, and traces every answer back to the source clause.

## Empirical evidence

The Aethis Engine achieves 100% across every benchmark section by construction. Frontier LLMs achieve high accuracy on most cases — and that accuracy is a *moving target*. Between March and April 2026 several specific failure cells in our v3.7 paper closed silently under the same model alias (e.g. GPT-5.4 on construction-CAR moved from 96.6% to 100% with no version bump). We test the architectural claim against three independent evidence sources:

1. **v3.8 adversarial CAR extension** — 20 newly-authored construction scenarios, current frontier models still fail (Claude Opus 4.7 18/20, GPT-5.4 default 19/20 with 0 reasoning tokens, Sonnet 4.6 19/20).
2. **External validation on LegalBench** — across 9 peer-reviewed tasks and 949 held-out cases authored by Stanford researchers, the engine is significantly more accurate than each of three frontier LLMs by combined paired-binomial McNemar's test (*p* < 0.001 vs Sonnet 4.6 and GPT-5.4; *p* = 0.003 vs Opus 4.7).
3. **The shifting-ground demonstration itself** — multiple v3.7 cells do not replicate at v3.8. For a regulated workflow that depends on benchmark-time accuracy claims, this property is structurally incompatible with verification frameworks like the EU AI Act. Deterministic execution avoids this class of risk by construction.

Full numbers, confidence intervals, statistical tests, and reproduction scripts:

- **Paper:** [*Confidently Wrong: Exception Chain Collapse in Frontier LLM Rule Evaluation*](https://github.com/Aethis-ai/confidently-wrong-benchmark/blob/main/paper/Simpson_Exception_Chain_Collapse_2026.md) — Simpson, Kozak, Doake, v3.8 (April 2026).
- **Benchmark + reproduction:** [confidently-wrong-benchmark](https://github.com/Aethis-ai/confidently-wrong-benchmark), including the [`legalbench/`](https://github.com/Aethis-ai/confidently-wrong-benchmark/tree/main/legalbench) external-validation harness and v3.8 reproducibility artefacts at `legalbench/docs/replication/`.

## Repositories

| Repo | Description |
|------|-------------|
| [confidently-wrong-benchmark](https://github.com/Aethis-ai/confidently-wrong-benchmark) | Benchmark dataset, paper, and reproduction scripts. 225 paper-scope scenarios across 4 domains, plus the [`legalbench/`](https://github.com/Aethis-ai/confidently-wrong-benchmark/tree/main/legalbench) external-validation harness on 9 LegalBench tasks (949 held-out cases) |
| [aethis-examples](https://github.com/Aethis-ai/aethis-examples) | Worked examples and test runner — run `uv run run_tests.py` with zero setup |
| [aethis-mcp](https://github.com/Aethis-ai/aethis-mcp) | MCP server — deterministic eligibility checks for any AI agent |
| [aethis-cli](https://github.com/Aethis-ai/aethis-cli) | Python CLI for the developer API |
| [aethis-sdk-python](https://github.com/Aethis-ai/aethis-sdk-python) | Official Python SDK for the developer API |

## Links

- [aethis.ai](https://aethis.ai) — Technology overview
- [aethis.legal](https://aethis.legal) — First application: UK immigration law
