# Markets

**K-12 edtech procurement is broken. Vendors own the information asymmetry.**

---

Districts make 3-5 year, multi-million dollar commitments based on marketing demos. Vendors know implementation complexity, actual performance data, and real contract terms. Districts get public marketing copy and a case study from an unlike district.

---

## GTM Execution

Outbound runs on governed workflows, not prompts. Every campaign is YAML-specced, budgeted, and checkpointed before a single agent fires. Human approval gates block sends.

**Email GTM**

Four voice segments, each with its own model, tone, and signal set:

| Segment | Role | Signal |
|---------|------|--------|
| EXEC | Superintendent | Budget efficiency, data-driven |
| INSTR | Instructional Lead | LCAP literacy initiatives, warmth |
| TECH | CTO / IT Director | SOC-2, Ed-Fi, concrete specs |
| SITE | Principal | "Save your teachers 3h/week" |

Content built on empirical priors: gratitude closings lift reply rate +38%, body length 50-125 words hits peak, reading grade 3rd-5th adds +36% lift. No external send without explicit human approval.

**Territory Intelligence**

Three agents cover California by enrollment: SoCal Large (>10K students), SoCal Small (<=10K), NorCal. Each scans 15 signal types (funding events, leadership changes, RFP activity, competitor signals) and routes to the appropriate workflow. Compliance enforcer runs as a pre-send gate on every pipeline: consent validation, FERPA scan, Brevo enforcement. Blocks sends that fail.

---

## Market Intelligence

### Vendor Evaluation

Districts make procurement decisions with marketing decks. Three agents run in parallel against each vendor:

- **Perplexity** extracts facts, validates citations, verifies pricing
- **Grok** runs sentiment analysis by role (teachers, admins, IT) and detects temporal trends
- **Gemini** detects contradictions across all three evidence streams and assigns risk scores

Examples of what the 3-agent swarm catches:

| Vendor Claim | Reality Found | Flag |
|---|---|---|
| "95% satisfaction" | Twitter sentiment: 0.4 | Unverified claim |
| "Proven results" | WWC: Moderate (2019, v2.1) | Version mismatch |
| "Zero false positives" | 40+ forum complaints | Contradiction |

Dossier per vendor in 2-4 minutes. Confidence scored by source count and recency weighting (under 90 days = 1.0x, over 180 days = 0.4x). Weekly refresh for watchlist vendors.

### Data Pipeline

50M+ rows from CDE, CAASPP, district filings, and compliance data. 59 source pipelines feeding CRM, email campaigns, and ML readiness scoring. Full architecture in [Building](./BUILDING.md).

### CRM Intelligence

12 object types, 605 attributes, 62 bilateral relationships, 24 filtered views, 19 action queues.

```
District (1,000+, 55-60 attrs)
  |-- School (10,000+, 40-45 attrs)
  |-- Person (3,600+, 52-57 attrs)  <- engagement level, PD progress, innovator flag
  |-- Implementation (health scores, renewal risk)
  +-- Signal (AI stage, PD status)
```

Queries return district briefings, influence-ranked stakeholder networks, and opportunity scores. New district profile triggers superintendent fetch, readiness tagging, and similar implementation surfacing automatically.

---

**1,000+ California districts profiled · 50M+ rows · 12 CRM object types · 605 attributes · 59 source pipelines**

**TypeScript · Go · Python · PostgreSQL · Supabase · Brevo · Attio · Claude API · OpenTelemetry**

---

[Building](./BUILDING.md) · [Orchestration](./ORCHESTRATION.md) · [Trust](./TRUST.md)

[jsunlee1013@gmail.com](mailto:jsunlee1013@gmail.com) · [LinkedIn](https://linkedin.com/in/jason-lee-60537831)
