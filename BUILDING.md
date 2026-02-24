# Building

**256+ tools, 35 servers, 74 agents. Every session needs a different slice of that.**

---

## Infrastructure

19 profile-scoped proxies. Each domain gets exactly the tools it needs, nothing more. Sales agents can't touch the database. Data agents can't send emails. The founder profile is the only one with full access. Reads are autonomous. Writes require human sign-off.

How profiles, skills, gates, and the governor work: [Orchestration](./ORCHESTRATION.md).

---

## Data Pipeline

California K-12 data is fragmented across 15+ incompatible formats. CAASPP alone is 31.96M rows. Building directly on raw data produces type errors, encoding failures, and broken joins within hours.

6-layer immutable pipeline with promotion gates (A-E). Failed rows quarantine automatically without blocking downstream. 50M+ rows processed. 254 standardized tables. 17 Attio serve views.

```
Ingest     -> stage (71 tables) + quarantine (failures isolated, downstream unblocked)
Transform  -> std (254 tables, 16 partitioned) + ref + dim + enrich
Serve      -> 17 Attio CRM views + 3 Brevo email views + 9 final published contracts
```

59 source pipelines. Pipeline outputs feed CRM, email campaigns, and ML readiness scoring directly.

```sql
-- Pseudo-nulls disguised as valid values, one of 25 quality checks
IF value IN ('NA', 'N/A', '-', 'NULL') THEN
  INSERT INTO ops.quarantine (reason) VALUES ('pseudo_null_detected');
  RETURN NULL;
END IF;
```

---

## Pigy

GTM control plane. No promotion without a proof receipt.

A Go simulator evaluates every proposal against a golden test suite before it ships. Receipts expire in 7 days. Correctness and fairness gates block promotions. Latency and cost are advisory. Live replay explicitly invalidates cached proofs.

```
SLM -> Simulator -> proof receipt -> Broker enforcement -> Operator review
```

9 operator pages. 26+ REST endpoints. 1,823 automated tests. 0 TypeScript errors.

---

## K-12 Classroom Agents

Ten agents for California educators. Teacher approval required on all of them.

| Agent | What it does |
|-------|-------------|
| Langston AI | Writing feedback that preserves student voice |
| Spiraly | Adaptive scheduling with student agency preserved |
| Soto | ELL silent period tools, comprehensible input before production |
| Norma | Rubric calibration with Bayesian IRR, bias detection |
| Exemplarable | Feedback forbidden without exact rubric citation |
| Reclass Tracker | EL reclassification evidence synthesis (CA Ed Code 313.3) |
| NGSS Alignment | 3D science alignment: DCI + SEP + CCC validation |
| Math Framework | 1,000-page CA Framework turned into a navigable semantic graph |
| Aidstep | FAFSA/CADAA tracking, March 2 deadline enforcement |
| Differentiation Engine | One worksheet turned into three differentiated versions |

No hallucinated examples. Every recommendation traceable to a source.

---

**256+ tools · 35 servers · 19 profiles · 50M+ rows · 1,823 Pigy tests · 10 K-12 agents**

**TypeScript · Go · Python · PostgreSQL · React · MCP · Claude API · OpenTelemetry**

---

[Markets](./MARKETS.md) · [Orchestration](./ORCHESTRATION.md) · [Trust](./TRUST.md)

[jsunlee1013@gmail.com](mailto:jsunlee1013@gmail.com) · [LinkedIn](https://linkedin.com/in/jason-lee-60537831)
