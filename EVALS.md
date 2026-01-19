# Evaluation & Testing

**Built a 10-dimension framework that makes agent failure visible and improvement measurable**

---

## Value Proposition

Built an end-to-end evaluation framework that scores agents 0-100 across 5 dimensions (Contract Clarity, Idempotency, Observability, Error Containment, Governance), gates autonomy progression via Trust Scorecard (TL0-TL4), and enforces 300+ automated checks before production deployment.

**Key Stats:**
- 10-dimension evaluation rubric (34 total points)
- 62 agents scored using 10-dimension rubric (verified: _leaderboard.md:19)
- 3 production-ready agents (≥28/34 score)
- 5-dimension Trust Scorecard (Contract Clarity, Idempotency, Observability, Error Containment, Governance)
- 300+ automated compliance checks
- TL0-TL4 autonomy gates (Draft → Manual → Sandbox → Supervised → Autonomous)

---

## Opening Hook

AI agents fail silently. I built a 10-dimension evaluation framework that makes failure visible and improvement measurable. From Extract→Audit→Test→Improve→Re-audit→Promote pipeline, every autonomy claim requires evidence. 62 agents scored across 34 points using 5-dimension Trust Scorecard. Only agents scoring ≥70 (TL3) run unsupervised; ≥90 (TL4) qualify for cross-system eligibility. Result: A quality gate that replaces "it works" with "it's proven."

---

## Core Capabilities

### Testing Methodology
- **10-Dimension Rubric:** Role, Tools, Context, Parallel, Concept, Think, Compact, Model, Clarity, Noonchi Integration (verified: _leaderboard.md:105)
- **34-Point Scoring:** Each dimension 0-4 points (verified: Total agents 62, audited 2026-01-11)
- **Golden vs. Aspirational Tests:** Pass/fail criteria, CI blocking on regression
- **Adversarial Fixtures:** 5 test scenarios (happy-path, error-handling, budget-exceeded, replan-loop, edge-adversarial) force failure modes

### Quality Gates
- **Automated Checks:** JSON Schema validation, idempotency tests (run twice, compare), health endpoint verification
- **Human Audit:** Security review, changelog completeness, edge case documentation
- **CI Integration:** No merge without passing Trust Scorecard thresholds
- **Promotion Gates:** TL0→TL1→TL2→TL3 auto-promote; TL3→TL4 requires human sign-off

### Evidence Collection
- **OpenTelemetry Tracing:** 19 event types (spawns, tool calls, routing, errors, completions)
- **Structured Logging:** NDJSON format with content hashes (no PII)
- **Audit Trails:** 7-year FERPA-compliant retention, chain hashing for tamper detection
- **Queryable Status:** `/health` endpoints expose real-time component health

### Continuous Improvement
- **Extract Phase:** Pull prompts from tier YAML, version to `Prompts/agents/{name}/v1.0.md`
- **Audit Phase:** Score against 10-dimension rubric using `_leaderboard.md` methodology
- **Test Phase:** Run adversarial fixtures, integration tests, fault injection
- **Improve Phase:** Fix gaps, update prompt, re-baseline performance
- **Re-Audit Phase:** Score again, compare to previous version
- **Promote Phase:** If score ≥ threshold, move to next autonomy level (TL0→TL4)

---

## Featured Projects

### 1. 10-Dimension Evaluation Rubric

**Problem:** Prompt quality is subjective. "Works" vs. "broken" doesn't measure production readiness. Need objective criteria that quantify autonomy risk.

**Solution:** Built 10-dimension rubric that scores each agent 0-4 per dimension (34 points total):

**10 Dimensions (0-4 each):**
1. **Role Definition:** Agent purpose, constraints, guardrails clearly specified
2. **Tool Use Quality:** What/When/When-Not guidance for all tools (CRITICAL gap for 53 skeleton agents)
3. **Context Management:** Memory setup, session state, compaction strategy
4. **Parallel Execution:** Explicit guidance on multi-step tool parallelization
5. **Conceptual Framing:** Domain knowledge (K-12, edtech, FERPA), heuristics present
6. **Extended Thinking:** Triggers for `<thinking>` blocks, complexity mapping
7. **Compaction Strategy:** Long-run memory persistence, checkpoint definitions
8. **Model Calibration:** Claude model selection (Haiku/Sonnet/Opus), fallback chains
9. **Clarity & Traceability:** Error messages, exit codes, debugging aids
10. **Noonchi Integration:** Memory namespaces, RAG setup, handoff points, audit logging

**Scoring Tiers (Full Prompts, n=9):**
- **Tier 1 (Production-Ready):** 28/34 = planner, researcher, reviewer (verified: _leaderboard.md:38-40)
- **Tier 1 (Needs Work):** 20-27/34 = coder, librarian, tester, sales-ops, signal-router, memory-manager (verified: _leaderboard.md:41-46)
- **Skeleton Prompts:** <20/34 = 53 agents with template-generated prompts (verified: _leaderboard.md:30, 47-99)

**Gap Analysis (Skeleton Prompts):**
- **CRITICAL (0/3 present):** Tool Use Quality (no WHAT/WHEN/WHEN NOT), Extended Thinking (no triggers), Parallel Execution (no guidance)
- **HIGH (0-1 present):** Model Calibration, Conceptual Framing
- **MEDIUM (2-3 present):** Noonchi Integration (only memory namespace present)

**Ownership Matrix:**
| Dimension | Owner | Method | Frequency |
|-----------|-------|--------|-----------|
| Role | Rex R (Reviewer) | Human audit vs charter | On PR |
| Tool Use | Tina T (Tester) | Automated + human review | CI |
| Context | Lily L (Librarian) | Schema validation + review | On change |
| Parallel | Ivan I (Integration) | Chaos test + review | CI |
| Concept | Rita R (Researcher) | Domain expert review | On PR |
| Thinking | Tina T (Tester) | Complexity mapping + test | CI |
| Compact | Maya M (Memory) | Long-run test + review | Weekly |
| Model | Quinn Q (QA) | Routing test + baseline | CI |
| Clarity | Rex R (Reviewer) | Error coverage audit | On PR |
| Noonchi | Maya M (Memory) | Integration test | CI |

**Impact:**
- Objective criteria replace subjective "it works" claims
- Production-ready agents (28/34) measurably different from skeleton agents (10/34)
- Gaps quantified: skeleton prompts lack 24-28 points on average
- Tier 1 agents within 1-3 points of production threshold (improvement path clear)

**Tech Stack:** JSON Schema, Python (evaluation harness), CI/CD
**Location:** `Evaluations/Agents/_leaderboard.md`, `Prompts/AUDIT-METRICS.md`

---

### 2. Trust Scorecard (5-Dimension Framework)

**Problem:** Eval scores ≠ autonomy readiness. An agent with a high prompt score could still crash in production if its contracts are undefined or error handling fails. Need infrastructure-level proof of reliability.

**Solution:** Built 5-dimension Trust Scorecard that gates autonomy levels (TL0-TL4) based on measurable reliability indicators:

**5 Dimensions (0-20 each = 0-100 total):**
1. **Contract Clarity (0-20):** I/O schemas exist, versioned, runtime-validated
   - 0-4: No schema, I/O undefined
   - 5-9: Schema exists but incomplete or unversioned
   - 10-14: Schema complete, versioned, not runtime-validated
   - 15-17: Schema complete, versioned, runtime-validated
   - 18-20: Schema + validated + edge case documentation

2. **Idempotency (0-20):** Re-execution produces identical results, retry-safe
   - 0-4: Unknown retry behavior
   - 5-9: Documented as "not retry-safe"
   - 10-14: Retry-safe for happy path only
   - 15-17: Retry-safe with idempotency key implemented
   - 18-20: Retry-safe, tested via CI, fault injection passed

3. **Observability (0-20):** Structured logs, health endpoints, error codes, status queryable
   - 0-4: No logging, no health endpoint
   - 5-9: Basic logging, unstructured
   - 10-14: Structured logs, no health endpoint
   - 15-17: Structured logs + health endpoint + error codes
   - 18-20: Full observability: logs, health, metrics, tracing, queryable status

4. **Error Containment (0-20):** Failures are graceful, scoped, do not cascade
   - 0-4: Errors crash or propagate unchecked
   - 5-9: Try/catch exists but no structured error response
   - 10-14: Structured errors, but no rollback defined
   - 15-17: Structured errors + rollback + blast radius documented
   - 18-20: Full containment: errors graceful, rollback tested, chaos tested

5. **Governance (0-20):** Owner assigned, changelog maintained, review gate passed
   - 0-4: No owner, no version, no changelog
   - 5-9: Owner exists but no versioning
   - 10-14: Owner + version but changelog incomplete
   - 15-17: Owner + version + changelog + review gate
   - 18-20: Full governance: owner, version, changelog, review, security sign-off

**Autonomy Thresholds:**
```
TL0 (0-24):   Draft          → Manual testing only
TL1 (25-49):  Assisted       → Human executes, agent advises
TL2 (50-69):  Supervised     → Human checkpoint at gates
TL3 (70-89):  Autonomous     → Unsupervised with logging (internal orchestration)
TL4 (90-100): Trusted        → Full autonomy, cross-system eligible (Manus-ready)
```

**Manus-Ready (TL4) Requirements:**
- Overall score ≥ 90
- Contract Clarity ≥ 85, Idempotency ≥ 80, Observability ≥ 90, Error Containment ≥ 80, Governance ≥ 85
- ≥ 50 successful executions in last 30 days
- Zero unhandled errors in last 30 days
- Contract version locked (not "latest")

**Scoring Automation:**
- **60% Automated:** JSON Schema validation, idempotency tests (run twice, compare), health checks, log validation
- **40% Human Audit:** Edge case documentation, changelog completeness, security sign-off

**Evidence Sources:**
- Schema files: `/contracts/{component}/`
- Idempotency tests: `/.ci/artifacts/{component}/idempotency.log`
- Health endpoint: `/{component}/health` (HTTP 200 + JSON)
- Fault injection results: `/.ci/artifacts/{component}/chaos.json`
- Owner file: `/{component}/OWNERS.yaml`
- Changelog: `/{component}/CHANGELOG.md`

**Runtime Adjustments:**
| Event | Impact | Recovery |
|-------|--------|----------|
| Successful execution | +0.1 (cap +5/period) | — |
| Handled error | No change | — |
| Unhandled error | -5 | +1/day after fix |
| Timeout | -3 | +1/day |
| Cascading failure | -20 | Human re-certification |
| Stale (30 days) | -2/week | Stops at -10; execution resets |

**Impact:**
- Objective proof replaces subjective confidence
- Autonomy progression gated by evidence, not promises
- Production incidents causally tied to dimension gaps (contracts undefined → -5 on unhandled error)

**Tech Stack:** PostgreSQL, Python, OpenTelemetry, CI/CD
**Location:** `docs/MASTERS/35-trust-scorecard.md`, `orchestrator/TRUST-CATALOGUE.md`

---

### 3. Observability Infrastructure & Adversarial Testing

**Problem:** Scoring agents means nothing if they fail silently in production. Need evidence collection at every level, plus forced failure testing to reveal hidden bugs.

**Solution:** Built dual-track observability:
- **Evidence Collection:** 19 event types emitted via OpenTelemetry to event bus
- **Adversarial Testing:** 5 fixture scenarios force specific failure modes

**19 Event Types (Observability):**
1. Agent spawned (tier, model, task_id)
2. Tool called (tool_name, agent_name, input schema)
3. Tool response received (output schema, latency)
4. Routing decision made (route_id, selected_model, confidence)
5. Replan triggered (reason: budget exceeded, tool failed, etc.)
6. Approval gate checked (gate_level, approved/denied)
7. Error encountered (error_type, error_code, agent_name)
8. Error handled (handler_name, action taken)
9. Fallback activated (fallback_model, reason)
10. Memory search executed (query, results_count)
11. Memory written (content_hash, visibility, pii_detected)
12. Write gate blocked (gate_name, reason)
13. Audit logged (action_type, actor, success/failure)
14. Health check run (endpoint, status_code, latency)
15. Task completed (task_id, agent_name, success, total_tokens)
16. Task failed (task_id, error_type, recovery_attempted)
17. Orchestrator event (state_change, timestamp)
18. Budget exceeded (agent_name, tokens_used, limit)
19. Rate limit hit (endpoint, retry_after)

**Adversarial Fixtures (5 Scenarios):**
1. **happy-path.ts:** All tools succeed, agent completes task (baseline)
2. **error-handling.ts:** Tool fails, agent recovery via fallback
3. **budget-exceeded.ts:** Token budget exhausted mid-task, expect graceful replan
4. **replan-loop.ts:** Tool returns invalid schema → replan → retry → success
5. **edge-adversarial.ts:** Multiple simultaneous failures (timeout + budget + schema error)

**Impact:**
- 300+ indicators feed into Trust Scorecard (verified: GOVERNANCE.md:303)
- Every production incident causally tied to observable event
- Adversarial tests reveal bugs before production (chaos engineering in CI)
- CI blocks merge if adversarial tests fail

**Tech Stack:** OpenTelemetry, TypeScript, Jest/Mocha, PostgreSQL
**Location:** `packages/orchestrator/src/__tests__/adversarial/fixtures/`, `packages/noonchi-telemetry/`

---

## Technical Depth

### Rubric Methodology
- **Audit Date:** 2026-01-11 (verified: _leaderboard.md:3)
- **Auditor:** claude-sonnet-4.5
- **Sample:** 9 full prompts (Tier 1) + 53 skeleton prompts (Tiers 0, 2-12)
- **Full Prompts:** Detailed audit against 10 dimensions
- **Skeleton Prompts:** Baseline audit of template-generated prompts
- **Average Score:** Full prompts 26.9/34, Skeleton prompts 10.0/34 (verified: _leaderboard.md:27-30)

### Contract-First Design
- **JSON Schema:** All agent inputs/outputs versioned and validated
- **Runtime Validation:** Incoming tool calls checked against schema before execution
- **Schema Drift Detection:** Changes logged, incompatibility gates deployments
- **Edge Case Documentation:** Contract contracts include constraints (max array length, min value, enum limits)

### Idempotency Implementation
- **Idempotency Keys:** Tool calls tagged with UUID to prevent duplicate side-effects
- **Deterministic Randomness:** Seeded RNG for reproducible outputs
- **State Snapshots:** Before/after comparison on retry
- **Replay Testing:** All agents must pass "run twice, get identical results" CI test

### Observability Pipeline
```
Event emission (agent code)
    ↓
OpenTelemetry SDK (batching, sampling)
    ↓
Event Bus (in-memory + persistent)
    ↓
Persister (NDJSON + PostgreSQL)
    ↓
Queryable via Trust Scorecard pre-flight API
    ↓
Dashboard aggregation
```

### Chaos Engineering in CI
- **Fault Injection:** Network timeouts, service degradation, resource exhaustion
- **Latency Injection:** Add 5s, 30s delays to tool calls
- **Response Corruption:** Return malformed JSON, missing fields
- **Cascade Testing:** Combine failures to test error propagation
- **Blast Radius:** Assert failures don't affect sibling agents

---

## Diagram Preview

**Eval Pipeline Flow:**
```
Extract → Audit → Test → Improve → Re-Audit → Promote
   ↓        ↓       ↓        ↓          ↓         ↓
 YAML     10-Dim   5x     Update     Score   TL0→TL4
 Pull     Rubric   Adv.    Prompt    Again    Gate
(1/62)    (34pts)  Fixtures (Fix)    (28/34)
```

**10-Dimension Rubric (Score Breakdown):**
```
Agent: planner
┌──────────────────────────────────────────────────────────┐
│ Role Definition       │████████║ 3/4                     │
│ Tool Use Quality      │████████║ 3/4                     │
│ Context Management    │████████║ 3/4                     │
│ Parallel Execution    │██████║░░ 2/4                     │
│ Conceptual Framing    │████████║ 3/4                     │
│ Extended Thinking     │██████║░░ 2/4                     │
│ Compaction Strategy   │██████║░░ 2/4                     │
│ Model Calibration     │████████║ 3/4                     │
│ Clarity & Tracing     │████████║ 3/4                     │
│ Noonchi Integration   │████║░░░░ 4/8 (COMMON GAP)        │
├──────────────────────────────────────────────────────────┤
│ TOTAL: 28/34 (82%) → PRODUCTION READY                    │
└──────────────────────────────────────────────────────────┘
```

**Trust Scorecard → Autonomy Progression:**
```
Score        Level    Gate          Permissions
───────────────────────────────────────────────────────
0-24         TL0      Manual Test   Development only
25-49        TL1      Human Exec    Advised mode
50-69        TL2      Checkpoint    Supervised gates
70-89        TL3      Logging ✅    Internal orchestration
90-100       TL4      Sign-off      Cross-system (Manus)
```

**Adversarial Test Coverage:**
```
happy-path           error-handling      budget-exceeded
(baseline)           (retry logic)       (replan trigger)
    ✓                    ✓                   ✓
    │                    │                   │
    └────────────────────┴───────────────────┘
                         ↓
        Edge Adversarial (multi-fail)
         Cascade + chaos + timeout
                    ✓
```

*Full diagrams available in portfolio and `docs/MASTERS/44-eval-harness.md`*

---

## Related Pages

- **[Governance & Compliance](./GOVERNANCE.md)** - How governance is tested through Trust Scorecard automation
- **[System Design & Architecture](./SYSTEMS.md)** - How observability infrastructure enables testing

---

## Call-to-Action

**Hiring for AI safety, ML testing, or quality engineering?**

The eval framework is battle-tested on 62 agents across 34 dimensions. Let's talk about making AI agents production-ready.

**Contact:** [jsunlee1013@gmail.com](mailto:jsunlee1013@gmail.com) | [LinkedIn](https://linkedin.com/in/jason-lee-60537831)

**Portfolio:** [jason-j-lee.com](https://jason-j-lee.com) - See all 25+ projects in interactive bento grid

---

## Code Samples

### 10-Dimension Rubric Scorer

```typescript
// Evaluations/Agents/_leaderboard.md scoring logic
interface AgentScore {
  agent: string;
  role: number; // 0-4
  tools: number;
  context: number;
  parallel: number;
  concept: number;
  think: number;
  compact: number;
  model: number;
  clarity: number;
  noonchi: number; // 0-8 (weighted 4/8 in total)
  total: number; // sum of all dimensions
}

function scoreAgent(prompt: string, charter: string): AgentScore {
  // 1. Extract role from charter
  const roleScore = hasRoleDefinition(charter) ?
    (hasConstraints(charter) && hasGuardrails(charter) ? 4 : 3) : 0;

  // 2. Score tool use (CRITICAL gap in skeletons)
  const toolsScore = hasToolGuidance(prompt) ?
    (hasWhatWhenWhenNot(prompt) ? 4 : 2) : 0;

  // 3. Score context management
  const contextScore = hasMemorySetup(prompt) ?
    (hasCompactionStrategy(prompt) ? 4 : 2) : 0;

  // ... continue for all 10 dimensions

  const total = roleScore + toolsScore + contextScore +
                parallelScore + conceptScore + thinkScore +
                compactScore + modelScore + clarityScore +
                (noonchiScore / 2); // weighted

  return { agent, role: roleScore, tools: toolsScore,
           context: contextScore, ..., total };
}
```

### Trust Scorecard Query (Pre-Flight)

```sql
-- Check if agent is TL3-eligible (internal orchestration)
SELECT component_id, overall_score, level
FROM ops.trust_scorecard
WHERE component_id = :agent_name
  AND overall_score >= 70  -- TL3 threshold
  AND contract_clarity >= 15
  AND idempotency >= 15
  AND observability >= 15
  AND error_containment >= 15
  AND governance >= 15;
-- Empty result = gate blocks deployment

-- Manus pre-flight (TL4-eligible, cross-system)
SELECT component_id, overall_score, level
FROM ops.trust_scorecard
WHERE component_id = :agent_name
  AND overall_score >= 90  -- TL4 threshold
  AND contract_clarity >= 85
  AND idempotency >= 80
  AND observability >= 90
  AND error_containment >= 80
  AND governance >= 85
  AND successful_runs_30d >= 50
  AND last_unhandled_error_at < NOW() - INTERVAL '30 days';
-- Manus integration depends on non-empty result
```

### Adversarial Test Fixture (Error Handling)

```typescript
// packages/orchestrator/src/__tests__/adversarial/fixtures/error-handling.ts

export const errorHandlingFixture = {
  name: "Error Handling Scenario",
  description: "Tool fails, agent recovers via fallback",

  steps: [
    {
      action: "spawn_agent",
      agent: "researcher",
      task: "Find enrollment trends for District 1234"
    },
    {
      action: "tool_fails",
      tool: "query_sql",
      error: "Connection timeout after 30s",
      expect: {
        agent_handles: true,
        fallback_triggered: "researcher → fallback_model",
        retry_attempted: true
      }
    },
    {
      action: "verify_observability",
      expect: {
        event_types: [
          "agent.spawned",
          "tool.called",
          "tool.failed",
          "error.handled",
          "fallback.activated",
          "tool.retried",
          "task.completed"
        ],
        audit_logged: true,
        pii_safe: true // no error content logged
      }
    },
    {
      action: "verify_containment",
      expect: {
        sibling_agents_running: true, // no cascade
        orchestrator_healthy: true,
        blast_radius: "researcher_only"
      }
    }
  ],

  assertions: [
    "final_status === 'completed'",
    "tokens_used <= budget",
    "error_handled === true",
    "observability_complete === true"
  ]
};
```

### Trust Scorecard Runtime Adjustment

```python
# packages/mcp-core/src/orchestrator/trust/scorecard.py

class TrustScorecard:
    async def record_execution(
        self,
        component_id: str,
        success: bool,
        error_type: Optional[str] = None
    ):
        """Record execution outcome and adjust score"""

        current = await self.get_score(component_id)

        if success:
            # Successful execution: +0.1 points (capped at +5/period)
            adjustment = min(0.1, 5 - current.period_gain)
            new_score = min(100, current.overall_score + adjustment)

        elif error_type == "unhandled_error":
            # Unhandled error: -5 points (recovery: +1/day)
            adjustment = -5
            new_score = max(0, current.overall_score + adjustment)

        elif error_type == "cascading_failure":
            # Cascading failure: -20 points (recovery: human re-cert)
            adjustment = -20
            new_score = max(0, current.overall_score + adjustment)
            alert = f"Cascading failure detected for {component_id}"

        elif error_type == "timeout":
            # Timeout: -3 points (recovery: +1/day)
            adjustment = -3
            new_score = max(0, current.overall_score + adjustment)

        # Log adjustment
        await self.audit_log(
            action="SCORE_ADJUSTED",
            component_id=component_id,
            previous_score=current.overall_score,
            adjustment=adjustment,
            new_score=new_score,
            reason=error_type or "execution_success"
        )

        # Check for auto-demotion
        level = self.score_to_level(new_score)
        if level < current.level:
            await self._notify_demotion(component_id, current.level, level)

        # Persist new score
        await self.update_score(component_id, new_score, level)
```

---

**Built with:** TypeScript, Python, PostgreSQL, Jest/Mocha, OpenTelemetry, JSON Schema, Claude Sonnet 4.5

**Repository:** [00-ACTIVE-MCP-SYSTEM](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM) (private - available on request)
