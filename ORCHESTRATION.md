# Orchestration

**74 agents across 13 tiers, all running through the same governor.**

---

## Governor

Every task runs inside a state machine: PENDING, RUNNING, PAUSED_FOR_USER, COMPLETE (and FAILED, RETRY, REPLANNING for failures). 10 states. 25 transitions. 15 boundary tests enforcing separation.

Five budget types cap every task:

```
Steps       -> max tool calls before forced review
Tokens      -> max context consumed
Replans     -> max strategy changes allowed
Wall-time   -> max elapsed time
Cost        -> max API spend
```

On every state transition the governor persists a checkpoint. On restart, all non-terminal tasks recover from the last checkpoint. No work lost on failure.

PAUSED_FOR_USER is a first-class state, not an error. High-risk actions trigger it before execution. Human approves, task resumes from the exact step it stopped.

---

## Workflow Patterns

Every workflow is YAML-specced: stages, gates, budgets, and role assignments defined before any agent fires. Three coordination patterns in production:

**Sequential, human send-gate** (email GTM campaigns)

```
Research -> Personalize -> Draft -> Score -> QA -> Approve -> Dispatch
```

Each stage is a separate model call with its own system prompt and tool allowlist. No external send without explicit human approval.

**Parallel fan-out, verifier gate** (PD content production)

```
Orchestrator -> [Spec Writer · Renderer · Scorer] + [Proofreader · Assembly]
             -> Verifier Gate -> Proofreader Gate -> final artifact
```

6 agents across 9 MCP servers (69 tools). Governor bounds retries and replans across all parallel streams simultaneously.

**Dual-path, mutation gate** (CRM sync)

```
Coordinator
  |-- Memory hygiene agent
  |-- Conflict resolution (Supabase <-> Attio sync)
  +-- Write consensus agent
-> Mutation gate -> committed + checkpointed
```

Read and write paths separated by role. All sync operations recoverable from the event log.

---

## Agent Architecture

74 agents across 13 tiers. Operators coordinate and delegate. Specialists execute narrow tasks.

| Tier | Focus |
|------|-------|
| T0 | Operators |
| T1 | Core ops (sales, data, PD, customer success) |
| T2 | Design (spec, architecture, pseudocode) |
| T3-T4 | Coordination and swarm |
| T5-T6 | Consensus and analysis |
| T7 | Testing |
| T8 | Data and ML |
| T9-T10 | Reasoning and development |
| T11-T12 | Neural and templates |

---

## Access Control

19 profile-scoped proxies. Each profile is a hard allowlist, not a suggestion. Sales agents can't touch the database. Data agents can't send email.

```
claude-sales  -> Attio + Brevo + email
claude-data   -> Supabase + Google + memory
claude-dev    -> Builder + skills
claude        -> All 35 servers (founder only)
```

Actions gated by approval class before execution:

| Gate | Type | What it covers |
|------|------|----------------|
| G0 | Read-only | Research, analysis, queries |
| G1 | Internal writes | Drafts, session state, local stores |
| G2 | External writes | Database, CRM, remote execution |
| G3 | Send operations | Email dispatch, API calls, irreversible actions |

G3 requires explicit human sign-off.

---

## Skills

133 skills wired across 19 profiles. Each skill is a system prompt fragment, a tool allowlist, and a dependency list. No ad-hoc capability. No implicit access.

Four deterministic tools manage the catalog (no LLM, all rule-based lookup):

| Tool | What it does |
|------|-------------|
| list_skills | Filtered catalog by profile, tier, risk, or tags |
| get_skill | Skill content + all resolved dependencies |
| match_task | Maps task to relevant skills via triggers and tags |
| check_deps | Validates dependency tree, detects cycles, reports missing |

T1 (41 skills, routine ops), T2 (70 skills, reasoning + ML), T3 (10 skills, builder-class, explicit authorization required). Profile gate, tier budget, risk budget, and circular dependency detection all enforce at lookup, not runtime.

---

## Context Engineering

8-layer semantic memory. Cache-first retrieval:

```
Request -> SQLite-vec (<10ms, local cache)
        | miss
        -> pgvector / Supabase (~50ms, cloud)
        | miss
        -> Relational fallback (Supabase Postgres)
        | manual
        -> Obsidian (founder-curated long-term knowledge)
```

Session layer: hooks load behavioral constraints at session start. Checkpoint system preserves agent state across context compaction. Agent layer: 5-tier working memory per agent with per-agent namespaces and handoff protocol.

---

**74 agents · 13 tiers · 19 profiles · 133 skills · 5 budget types · G0-G3 gates**

**TypeScript · Go · Python · PostgreSQL · MCP · Claude API · OpenTelemetry**

---

[Markets](./MARKETS.md) · [Building](./BUILDING.md) · [Trust](./TRUST.md)

[jsunlee1013@gmail.com](mailto:jsunlee1013@gmail.com) · [LinkedIn](https://linkedin.com/in/jason-lee-60537831)
