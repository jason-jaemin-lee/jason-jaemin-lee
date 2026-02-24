# Trust

**74 agents. 10 dimensions. 34 points.**

---

## The Rubric

10 dimensions, 34 total points. Production-ready threshold: 28/34.

| Dimension | Pts | What it catches |
|-----------|-----|-----------------|
| Role Definition | 3 | Vague purpose = unpredictable behavior |
| Tool Use Quality | 3 | When-not-to-use matters as much as when |
| Context Structure | 3 | No compaction plan = context collapse at scale |
| Parallel Tool Use | 2 | Sequential by default = 3x slower |
| Conceptual Framing | 3 | Missing domain knowledge (FERPA, EL law) = wrong outputs |
| Extended Thinking | 3 | Triggers missing = shallow reasoning on hard problems |
| Compaction Strategy | 3 | Checkpoint gaps = lost work on long runs |
| Model Calibration | 3 | Haiku for Opus work = failures |
| Clarity & Anti-Hedging | 3 | Hedging language in ops context = inaction |
| Noonchi Integration | 8 | Memory, contracts, CA compliance, founder style |

What the rubric exposed: Extended Thinking is the weakest dimension across every tier. Compaction Strategy scores 1-2/4 universally. Parallel Execution is boilerplate in most specs. Full evaluation pending.

---

## Three Evaluation Domains

**Cognitive** (34-pt rubric above): agent behavior quality per spec.

**Systemic** (0-100 trust scorecard): infrastructure reliability across five dimensions: Contract Clarity, Idempotency, Observability, Error Containment, Governance. Score maps to TL0 (dev only) through TL4 (full autonomy, cross-system eligible). 60% automated, 40% human audit.

**Behavioral** (4-layer harness): compliance (binary pass/fail, 0 violations target), routing (>95% accuracy), cognitive (LLM-judge), tool I/O (100% deterministic). FERPA compliance checks run at Layer 1 for every agent that touches student data.

---

## Trust Levels

Five levels, scored on five dimensions (0-20 each, 0-100 total).

| Level | Score | What it means |
|-------|-------|---------------|
| TL4 | 90-100 | Full autonomy, cross-system eligible |
| TL3 | 70-89 | Unsupervised with logging |
| TL2 | 50-69 | Human checkpoint at critical junctures |
| TL1 | 25-49 | Human executes, agent advises |
| TL0 | 0-24 | Not deployable |

Execution trust is capped by the weakest dependency in the call chain. A TL4 workflow invoking a TL2 skill runs at TL2 for that execution.

---

## Effective Trust Level (v2, proposed)

The base scorecard (CC/ID/OB/EC/GV) is live and unchanged. Three additional caps are proposed but not yet enforced in CI or runtime.

```
effective_tl = min(
  baseline_tl,        -- CC/ID/OB/EC/GV score (live, ops.trust_scorecard)
  ps_gate_cap,        -- Policy/Safety hard gate
  rq_role_cap,        -- Role Quality cap
  eo_review_cap       -- Educational Outcomes cap
)
```

**PS (Policy/Safety).** Hard blocker. An active BLOCKER violation forces TL0 regardless of baseline score. An agent can score 95/100 and still be blocked. Violations stored in `ops.policy_violation` with severity BLOCKER or WARNING. Zero active blockers = PS passes.

**RQ (Role Quality).** Role-typed, not generic. Each agent declares a role (planner, coordinator, executor, reviewer) and is scored against that role's criteria. A coordinator that plans well but coordinates poorly fails RQ even if its aggregate numbers look fine.

**EO (Educational Outcomes).** Human-reviewed. Qualified reviewers (credentialed teachers, district admins) score agent outputs on clarity, actionability, equity/inclusion, and instructional alignment. Quarterly review window, minimum sample size required. This is the cap that keeps the system honest about whether agents actually help educators.

All three dimensions have schema (migration 018) and documentation (36-standards.md sections 19-21). None are wired into deployment gates yet.

---

## PD RAG Benchmark

3-layer evaluation on the Professional Development RAG system (1,213 chunks):

| Metric | Result | Target |
|--------|--------|--------|
| Success@10 | 95.38% | >=85% |
| MRR | 0.820 | >=0.70 |
| Generation Quality | 90/100 | - |

8/8 gates testable, 0% critical violation rate.

---

## Why K-12 Is Different

IEP and 504 flags are CRITICAL in the PII detector, not HIGH. An IEP is a protected health record under FERPA and IDEA. A system that treats it as generic student data has never been in a special education meeting. That kind of domain knowledge has to be in the design from day one.

---

## 7 Write Gates

Every memory write passes 7 gates: PII scan (22 patterns, CRITICAL/HIGH/MEDIUM tiers), provenance check, type classification, SHA256 dedup, ABAC band authorization, size limits, injection pattern rejection.

CRITICAL tier blocks the write entirely. HIGH tier scrubs content. MEDIUM tier scrubs silently. IEP, 504, SSN, DOB are all CRITICAL.

---

## FERPA Audit Trail

Chain-hashed append-only log. SHA256(previous_hash + entry_json). If any entry is modified, all subsequent hashes break. No PII in logs, only hashes, timestamps, actions, categories. Daily rotation. 7-year retention (hot 90d, warm 1yr, cold 6yr S3 Glacier).

---

## Capability Position

| Status | What |
|--------|------|
| Ahead | Tool isolation via profile allowlists before execution, not at runtime |
| Ahead | Task governance: state machine with budgets, pause/resume, gate-aware workflows |
| Comparable | Audit and forensics, operator control plane |
| Closing | Git shared-state safety, system-wide freeze/snapshot/undo, bypass detection |

---

**74 agents · 10-dimension rubric · ~300 binary-testable indicators · 7 write gates · 7-year FERPA retention · TL0-TL4 + PS/RQ/EO caps**

---

[Markets](./MARKETS.md) · [Building](./BUILDING.md) · [Orchestration](./ORCHESTRATION.md)

[jsunlee1013@gmail.com](mailto:jsunlee1013@gmail.com) · [LinkedIn](https://linkedin.com/in/jason-lee-60537831)
