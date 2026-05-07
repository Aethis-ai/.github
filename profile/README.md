<div align="center">

# Aethis

<!-- aethis-bible: public-messaging.md#2-headlines-ranked -->
**Deterministic decisions for regulated AI.**

Compile legislation, policy, and contracts into formal logic. Get the same correct answer every time, with a full audit trail back to the source clause — no LLM in the decision path.

[![Docs](https://img.shields.io/badge/Docs-docs.aethis.ai-0A66C2?logo=readthedocs&logoColor=white)](https://docs.aethis.ai) &nbsp; [![License: MIT](https://img.shields.io/badge/License-MIT-2ea44f.svg)](https://opensource.org/licenses/MIT)

</div>

---

> **Authoring is in private beta.** Decision tools (`decide`, `fields`, `explain`, `aethis_decide`, `aethis_schema`) are public — no key required. Authoring tools (rule generation, test refinement, publishing) require an invite. Request access at [aethis.ai/developer-access](https://aethis.ai/developer-access).

## Install

| Package | Install | Version |
|---|---|---|
| **`aethis-mcp`** &nbsp; drop-in MCP server for Claude Code, Cursor, Windsurf | `npx -y aethis-mcp` | [![npm](https://img.shields.io/npm/v/aethis-mcp.svg?label=&logo=npm&logoColor=white&color=CB3837)](https://www.npmjs.com/package/aethis-mcp) |
| **`aethis-sdk`** &nbsp; Python SDK | `pip install aethis-sdk` | [![pypi](https://img.shields.io/pypi/v/aethis-sdk.svg?label=&logo=python&logoColor=white&color=3776AB)](https://pypi.org/project/aethis-sdk/) |
| **`aethis-cli`** &nbsp; author and publish rulesets | `uv tool install aethis-cli` | [![pypi](https://img.shields.io/pypi/v/aethis-cli.svg?label=&logo=python&logoColor=white&color=3776AB)](https://pypi.org/project/aethis-cli/) |

---

## Quick start

### MCP — drop into Claude Code, Cursor, Windsurf

```bash
claude mcp add aethis -- npx -y aethis-mcp
```

Once registered, your agent has a deterministic `aethis_decide` / `aethis_explain` / `aethis_schema` / `aethis_next_question` toolset. Worked example:

```text
> A £250M construction project lodges an access-damage claim caused by
  removing a design-defective component. Does CAR cover apply?

⚒  aethis_decide(
     ruleset_id="aethis/construction-all-risks",
     field_values={
       "car.policy.period_valid": true,
       "car.claim.is_access_damage": true,
       "car.defect.origin": "design",
       "car.project.value_millions_gbp": 250,
     },
   )

   decision    : not_eligible
   ruleset     : aethis/construction-all-risks
   engine      : aethis-core@0.10.0  (no LLM in decision path)
   trace       :
     satisfied      policy_period            (Cl.3)
     satisfied      access_exclusion         (Cl.8)   access damage triggers exclusion
     satisfied      enhanced_cover           (Cl.9.1) ≥£100M would normally reinstate
     not_satisfied  carveback_qualification  (Cl.9.2) design-defect carveback bites
     not_satisfied  pioneer_override         (Cl.9.3) project < £500M, no override
```

The engine asked four of eleven questions and short-circuited.

### Python SDK — embed in your service

```bash
pip install aethis-sdk
```

```python
from aethis_sdk import Aethis

with Aethis() as client:
    result = client.decide(
        ruleset_id="aethis/construction-all-risks",
        field_values={
            "car.policy.period_valid": True,
            "car.claim.is_access_damage": True,
            "car.defect.origin": "design",
            "car.project.value_millions_gbp": 250,
        },
    )
    print(result.decision)   # → "not_eligible"
    print(result.slug)       # → "aethis/construction-all-risks"
```

`ruleset_id` accepts a slug or a versioned ID. Pass `api_key=` to `Aethis(...)` once you have an authoring key.

### CLI — author your own rulesets

```bash
uv tool install aethis-cli
aethis init my-rules
```

`aethis init` walks you through compiling a rulebook from prose to a deployable ruleset, with tests generated from the source text. See the [authoring guide](https://docs.aethis.ai).

### Try it without installing anything

`decide` is on the public REST API. No key, no SDK:

```bash
curl -s https://api.aethis.ai/api/v1/public/rulesets | jq '.[].slug'
# "aethis/construction-all-risks"
# "aethis/consumer-credit-prequalification"
# "aethis/spacecraft-crew-certification"
# …

curl -s -X POST https://api.aethis.ai/api/v1/public/decide \
  -H "Content-Type: application/json" \
  -d '{
    "ruleset_id": "aethis/construction-all-risks",
    "field_values": {
      "car.policy.period_valid": true,
      "car.claim.is_access_damage": true,
      "car.defect.origin": "design",
      "car.project.value_millions_gbp": 250
    }
  }' | jq '.decision'
# "not_eligible"
```

---

## How it works

```
┌───────────────────┐    once, at authoring time    ┌──────────────────┐
│ Source legislation│ ──────────────────────────▶   │  Formal logic    │
│ / policy /contract│   (LLM-assisted compile)      │ (SMT-backed DSL) │
└───────────────────┘                               └────────┬─────────┘
                                                             │
                                                      publish ruleset
                                                             │
                                                             ▼
            ┌────────────────────────────────────────────────────────────┐
            │  decide(ruleset, field_values) → decision + trace + schema │
            <!-- aethis-bible: claims.md#latency -->
            │  • <1ms median decision                                    │
            │  • no LLM in the path                                      │
            │  • audit trail back to source clause                       │
            └────────────────────────────────────────────────────────────┘
```

The engine asks the **smallest** set of questions needed to reach a decision (constraint-driven question selection), short-circuits as soon as the answer is determined, and exposes a stable schema your UI or agent can drive.

## Public repos

| Repo | What it is |
|---|---|
| [**confidently-wrong-benchmark**](https://github.com/Aethis-ai/confidently-wrong-benchmark) | The Simpson 2026 paper, the 225-scenario benchmark across four domains, and the LegalBench external-validation harness (949 held-out cases, 9 tasks). Reproducible end-to-end. |
| [**aethis-examples**](https://github.com/Aethis-ai/aethis-examples) | Runnable example rulesets. `uv run run_tests.py` and you'll see decisions trace through real rules in seconds. |
| [**aethis-skills**](https://github.com/Aethis-ai/aethis-skills) | Agent skills for Claude Code and other agentic IDEs — pre-built workflows for authoring rulesets, debugging, and publishing. |

## Research

> [**Confidently Wrong: Exception Chain Collapse in Frontier LLM Rule Evaluation**](https://github.com/Aethis-ai/confidently-wrong-benchmark/blob/main/paper/Simpson_Exception_Chain_Collapse_2026.md) — Simpson, Kozak, Doake (v3.8, April 2026).

<!-- aethis-bible: claims.md#internal-benchmark-aethis-vs-frontier-llms-225-scenarios -->
Engine accuracy: 100% across 225 scenarios spanning four rule domains, where frontier LLMs score 63–100% (Simpson 2026 §3). External validation on Stanford's LegalBench: significantly more accurate than Claude Opus 4.7 and GPT-5.4 across 9 tasks and 949 held-out cases (Simpson 2026 §6.10).

Reproducible benchmark + LegalBench harness: [Aethis-ai/confidently-wrong-benchmark](https://github.com/Aethis-ai/confidently-wrong-benchmark).

## Links

- **[docs.aethis.ai/agents](https://docs.aethis.ai/agents)** — onboarding for AI coding agents (Claude Code, Cursor, Windsurf): install + verify + auth + workflow patterns in one page.
- **[docs.aethis.ai](https://docs.aethis.ai)** — developer docs, API reference, authoring guide.
- **[aethis.ai](https://aethis.ai)** — platform overview and developer-access requests.
- **[aethis.legal](https://aethis.legal)** — first vertical application: UK immigration law.

---

<div align="center">
<sub>Built in London ❤️ &nbsp;·&nbsp; <a href="https://aethis.ai/developer-access">Request developer access →</a></sub>
</div>
