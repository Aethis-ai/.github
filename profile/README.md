# Aethis

**LLMs interpret rules. Aethis compiles them.**

AI agents are making eligibility and compliance decisions using LLM reasoning. Most of the time it works. When it doesn't, nobody notices — the model returns a confident, well-structured wrong answer with no audit trail. In regulated workflows, decisions must be deterministic, explainable, and reproducible. LLMs fail all three regardless of peak accuracy.

## How it works

An LLM compiles source legislation into formal logic once, at authoring time. After that, every decision is pure constraint evaluation — no LLM in the decision path. The engine finds the optimal question to ask next, short-circuits when a decision is reachable, and traces every answer back to the source clause.

## Empirical evidence

The Aethis Engine achieves 100% across every section of our published benchmark while no frontier LLM tested matches that on the deepest exception-chain domain. We further validated externally on [LegalBench](https://hazyresearch.stanford.edu/legalbench/) — the standard peer-reviewed legal-reasoning benchmark — across 9 tasks and 949 held-out cases authored by Stanford researchers, where the engine is significantly more accurate than each of three frontier LLMs.

Full numbers, confidence intervals, statistical tests, and reproduction scripts:

- **Paper:** [*Confidently Wrong: Exception Chain Collapse in Frontier LLM Rule Evaluation*](https://github.com/Aethis-ai/confidently-wrong-benchmark/blob/main/paper/Simpson_Exception_Chain_Collapse_2026.md) — Simpson, Kozak, Doake (April 2026).
- **Benchmark + reproduction:** [confidently-wrong-benchmark](https://github.com/Aethis-ai/confidently-wrong-benchmark), including the [`legalbench/`](https://github.com/Aethis-ai/confidently-wrong-benchmark/tree/main/legalbench) external-validation harness.

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
