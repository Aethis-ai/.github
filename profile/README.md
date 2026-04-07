# Aethis

**LLMs interpret rules. Aethis compiles them.**

AI agents are making eligibility and compliance decisions using LLM reasoning. Most of the time it works. When it doesn't, nobody notices — the model returns a confident, well-structured wrong answer with no audit trail.

We tested frontier LLMs on 11 insurance coverage scenarios with a five-level exception chain:

| | Accuracy | Speed | Deterministic | Explainable | Regulated use |
|--|----------|-------|:---:|:---:|:---:|
| **Aethis Engine** | **100%** | **<5ms** | ✅ | ✅ | ✅ |
| GPT-5.4 | 100% | 2-5s | ❌ | ❌ | ❌ |
| Claude Opus 4.6 | 100% | 2-5s | ❌ | ❌ | ❌ |
| Claude Sonnet 4.6 | 91% | 1-3s | ❌ | ❌ | ❌ |
| GPT-5.4-mini | 82% | 1-2s | ❌ | ❌ | ❌ |
| GPT-5.3 (ChatGPT) | 27% | 2-5s | ❌ | ❌ | ❌ |

Flagship LLMs match the engine on accuracy — but accuracy is table stakes. In regulated workflows, decisions must be deterministic, explainable, and non-stochastic. LLMs fail all three regardless of accuracy. The model most people use daily (GPT-5.3) gets 73% wrong.

[Full benchmarks and reproducible tests →](https://github.com/Aethis-ai/aethis-examples)

## How it works

An LLM compiles source legislation into formal logic once, at authoring time. After that, every decision is pure constraint evaluation — no LLM in the decision path. The engine finds the optimal question to ask next, short-circuits when a decision is reachable, and traces every answer back to the source clause.

## Repositories

| Repo | Description |
|------|-------------|
| [aethis-examples](https://github.com/Aethis-ai/aethis-examples) | Benchmarks, examples, and test runner — run `uv run run_tests.py` with zero setup |
| [aethis-mcp](https://github.com/Aethis-ai/aethis-mcp) | MCP server — deterministic eligibility checks for any AI agent |
| [aethis-cli](https://github.com/Aethis-ai/aethis-cli) | Python CLI for the developer API |

## Links

- [aethis.ai](https://aethis.ai) — Technology overview
- [aethis.legal](https://aethis.legal) — First application: UK immigration law
