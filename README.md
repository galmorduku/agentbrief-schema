# AgentBrief Schema

**A PM-grade specification format for AI agents.**

`agentbrief.yaml` is what your agent is held accountable to — identity, mission, scope, behavior, guardrails, tools, compliance, and more. One file, 15 sections, Apache-2.0.

> *"Blogs are read, specs are executed."*

---

## Why a spec?

Most agents in production right now were vibe-coded into existence. There is a prompt, a few tools, and a hope. The team tests outputs. Nobody can explain how the agent actually decides anything. When it fails, nobody knows where to look.

AgentBrief is the artifact that fixes that. A YAML spec that lives in your repo, gets diffed in PRs, and is the thing you point to when something goes wrong.

---

## The 6-question framework

Every one of the 15 schema sections is owned by exactly one question. No orphans.

| Question | Sections covered |
|---|---|
| Q1 — What is this agent? | `identity` |
| Q2 — What does it do, and what does it never do? | `mission` + `scope` |
| Q3 — How does it decide, and what tools can it touch? | `behavior` + `guardrails` + `tools` |
| Q4 — When does it stop, escalate, or remember? | `escalation` + `trust` + `memory` |
| Q5 — How do we know it is working? | `evals` + `observability` + `compliance` |
| Q6 — Who owns it, when does it change, and what does it cost? | `versioning` + `lineage` + `sla` |

---

## Sections

**Required (5):** `identity` · `mission` · `scope` · `behavior` · `guardrails`

**Optional (10):** `tools` · `escalation` · `evals` · `observability` · `compliance` · `versioning` · `lineage` · `memory` · `trust` · `sla`

Optional sections carry a `_maturity` field (`draft` | `reviewed` | `production`) so you can ship an incomplete spec without blocking.

---

## Quickstart

```yaml
# Q1 — What is this agent?
agentbrief_schema: "1.0"

identity:
  name: "Support Agent"
  version: "1.0.0"
  type: assistant
  description: "Handles tier-1 customer support for a SaaS product."
  owner: "product@company.com"

# Q2 — What does it do, and what does it never do?
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
  primary_users:
    - Free users
    - Paid subscribers

# Q3 — How does it decide, and what tools can it touch?
behavior:
  default_mode: "Follow playbook; ask one clarifying question before acting"
  uncertainty_handling: "Acknowledge, offer top-2 answers, offer to escalate"

guardrails:
  never:
    - Share another customer's account data
    - Process refunds over $500 without human approval
  require_confirmation:
    - Any action that modifies account settings
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

| Framework | Covered sections |
|---|---|
| NIST AI RMF (Agentic Profile) | `compliance`, `evals`, `guardrails`, `observability` |
| EU AI Act | `compliance`, `identity`, `guardrails` |
| DeepEval / Promptfoo / Braintrust | `evals` |
| LangSmith / Galileo / Ragas | `observability`, `evals` |
| GAIA / AgentBench / OSWorld / BFCL | `evals.benchmark_targets` |
| Sensei (Monday.com) | `mission`, `evals`, `behavior`, `memory` |
| AIUC-1 (6 domains) | `compliance.aiuc1` |

---

## Tooling

[agentbrief.duku.xyz](https://agentbrief.duku.xyz?utm_source=github) — guided authoring, 1-click validation, compliance scoring. Join the waitlist for early access.

The schema is open. The tooling is the product.

---

## License

Apache-2.0. Use it, fork it, build on it.

---

If this schema is useful to you, a star helps others find it.
