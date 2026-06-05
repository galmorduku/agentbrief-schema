# AgentBrief Schema

**A PM-grade specification format for AI agents.**

`agentbrief.yaml` is what your agent is held accountable to — identity, mission, scope, behavior, guardrails, tools, compliance, and more. One file, 15 sections, Apache-2.0.

> *"Blogs are read, specs are executed."*

---

## Why a spec?

Most agents ship with a system prompt and a README. Neither is structured enough to validate, audit, or hand off. AgentBrief gives you a machine-parseable spec that covers the surfaces that actually cause production failures: undeclared scope, missing escalation paths, untested evals, and compliance blind spots.

---

## The 4-question framework

| Question | Sections covered |
|---|---|
| Q1 — Break it down into operations | `mission` + `scope` |
| Q2 — Decision rules | `behavior` + `guardrails` |
| Q3 — Failure points | `evals` + `escalation` + `observability` |
| Q4 — Self-examination | `lineage` + `versioning` |

---

## Sections

**Required (5):** `identity` · `mission` · `scope` · `behavior` · `guardrails`

**Optional (10):** `tools` · `escalation` · `evals` · `observability` · `compliance` · `versioning` · `lineage` · `memory` · `trust` · `sla`

Optional sections carry a `_maturity` field (`draft` | `reviewed` | `production`) so you can ship an incomplete spec without blocking.

---

## Quickstart

```yaml
agentbrief_schema: "1.0"

identity:
  name: "Support Agent"
  version: "1.0.0"
  type: assistant
  description: "Handles tier-1 customer support for a SaaS product."
  owner: "product@company.com"

mission:
  goal: "Resolve common support requests without human escalation."
  north_star: "< 2 min median resolution time, > 90% CSAT"
  success_criteria:
    - "Resolves billing, account, and onboarding questions autonomously"
    - "Escalates edge cases with full context"

scope:
  in_scope:
    - Billing questions
    - Account recovery
    - Onboarding walkthroughs
  out_of_scope:
    - Legal or compliance advice
    - Custom contract negotiations
  user_types:
    - Free users
    - Paid subscribers

behavior:
  tone: professional and concise
  decision_policy: >
    Follow the support playbook. When in doubt, ask one clarifying
    question before acting. Never assume intent.
  fallback: "Acknowledge, log, and escalate to human agent."
  language: en

guardrails:
  hard_stops:
    - Never share another customer's account data
    - Never process refunds over $500 without human approval
  pii_handling: redact
  content_policy: company-standard-v2
```

---

## Full schema

The schema is defined in [`schema/v1.yaml`](schema/v1.yaml) using JSON Schema (YAML syntax). It validates with any JSON Schema Draft 2020-12 validator.

**Validate your spec:**

```bash
# Node
npx ajv validate -s schema/v1.yaml -d your-agent.agentbrief.yaml --spec=draft2020

# Python
pip install jsonschema pyyaml
python3 -c "
import yaml, jsonschema
schema = yaml.safe_load(open('schema/v1.yaml'))
doc    = yaml.safe_load(open('your-agent.agentbrief.yaml'))
jsonschema.validate(doc, schema)
print('Valid')
"
```

---

## Eval framework coverage

The schema is designed to map directly to the major agent evaluation frameworks:

| Framework | Covered sections |
|---|---|
| NIST AI RMF (Agentic Profile) | `compliance`, `evals`, `guardrails`, `observability` |
| EU AI Act | `compliance`, `identity`, `guardrails` |
| DeepEval / Promptfoo / Braintrust | `evals` |
| LangSmith / Galileo / Ragas | `observability`, `evals` |
| GAIA / AgentBench / OSWorld / BFCL | `evals.benchmark_targets` |
| AIUC-1 (6 domains) | `compliance.aiuc1` |

---

## Tooling

[agentbrief.duku.xyz](https://agentbrief.duku.xyz?utm_source=github) — web editor, 1-click validation, compliance scoring, and PDF export.

The schema is open. The tooling is the product.

---

## License

Apache-2.0. Use it, fork it, build on it.

---

If this schema is useful to you, a star helps others find it.
