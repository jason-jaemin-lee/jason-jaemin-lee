# System Design & Architecture

**Orchestrated 100+ tools across 20 servers with governance at every layer**

---

## Value Proposition

Built multi-tier orchestration infrastructure that processes 60M rows, coordinates 100+ tools across 20 MCP servers, and maintains <100ms routing latency — all while enforcing approval gates, idempotency, and trust-based autonomy.

**Key Stats:**
- 100+ MCP tools across 20+ servers
- 60M+ rows processed (Data Pipeline)
- 66ms routing latency (verified: portfolio/src/components/ProfessionalLearningPage.tsx:254)
- 18 profile-scoped access controls (lazy-MCP proxies)

---

## Opening Hook

Orchestrating 100+ tools across 20 servers without everything breaking requires discipline. I built multi-tier approval gates (G0-G3), ML-optimized routing, and idempotent pipelines that process 60M rows of California K-12 data. The Trust Scorecard ensures no agent runs unsupervised until it proves reliability across 300+ checks. Result: A system that scales from draft-mode tools (TL0) to fully autonomous agents (TL4) with governance at every layer.

---

## Core Capabilities

### Multi-Tier Orchestration
- **Foundation Layer (GREEN):** 100+ MCP tools, 20+ servers organized into operational domains (Sales, Data, Builder, Research, Memory, Routing)
- **Intelligence Layer (YELLOW):** Attio-Supabase bidirectional sync, Data Enrichment pipeline with 25 quality checkpoints
- **Applications Layer (BLUE):** CRAFT framework, Vendor Evaluation, Professional Development systems

### Data Engineering at Scale
- **60M+ rows processed** across 1000+ California districts, 10,000 schools
- **6-layer architecture:** Raw → Stage → Std → Enrich → Semantic → Final
- **25 quality checkpoints:** Gates A-E enforce encoding, types, referential integrity, business rules, contracts
- **Idempotent design:** All writes are recoverable, supports dry-run mode

### Performance Optimization
- **66ms routing latency:** Low-latency model switching verified in production (ProfessionalLearningPage.tsx:254)
- **Lazy tool loading:** 18 profile-scoped proxies expose only needed tools per context, preventing tool overload
- **Query optimization:** Materialized views (`ops.v_runs`) with composite indexes for fast control plane queries

### Infrastructure Patterns
- **Approval Gates (G0-G3):** Read-only → Internal writes → DB/CRM writes → External send
- **Least-Privilege Profiles:** 18 profile-scoped access controls (research, sales-draft, crm-write, etc.)
- **Circuit Breakers:** Retry logic, dead-letter queues, graceful degradation
- **Observability:** OpenTelemetry tracing, 19 event types, audit logs with 7-year retention

---

## Featured Projects

### 1. AI-Native Orchestration Stack

**Problem:** Coordinating 100+ tools across 20+ servers without a single entrypoint leads to tool sprawl, permission confusion, and ungoverned side-effects.

**Solution:** Built 3-tier orchestration system with Model Context Protocol (MCP):
- **Foundation (GREEN):** 100+ tools organized into 20+ servers (Google Workspace, Supabase, Attio CRM, Brevo Email, Memory Service, etc.)
- **Intelligence (YELLOW):** Bidirectional sync, ML prefetching, data enrichment
- **Applications (BLUE):** CRAFT framework, Vendor Eval, PD systems

**Architecture:**
```
Claude Code → 18 lazy-* proxies → Profile-scoped allowlists → MCP servers → External APIs
           ↓
    Lazy loading: Tools load on first use, not at startup
```

**Impact:**
- Single Claude Code session can access 100+ tools through consistent interface
- Least-privilege enforcement via profile-scoped proxies (research, crm-write, sales-send)
- Lazy loading prevents tool schema bloat, enables larger context windows for complex tasks

**Tech Stack:** TypeScript, Model Context Protocol, FastAPI, PostgreSQL, Node.js
**Location:** `packages/orchestrator/`, `.claude/lazy-mcp/`, `packages/mcp-core/`

---

### 2. Data Pipeline Architecture

**Problem:** California K-12 data arrives in 15+ incompatible formats (CDE enrollment CSVs, CAASPP assessments, district PDFs, web scraped vendor data). Building on raw data leads to encoding errors, type mismatches, and broken joins.

**Solution:** Built 6-layer immutable pipeline with promotion gates:

**Layers:**
1. **Raw:** Write-once ingest with file hash verification
2. **Stage:** Type coercion, trimming, encoding fixes (UTF-8 normalization)
3. **Std:** Reference/crosswalks, unit normalization, 14-digit CDS codes
4. **Enrich:** Entity resolution, joins, MDM-lite (district/school/person matching)
5. **Semantic:** Ontology, policy rules, business logic
6. **Final:** Contracted, versioned serving layer with RLS

**Quality Gates:**
- **Gate A (Raw → Stage):** File hash match, rowcount > 0, encoding OK
- **Gate B (Stage → Std):** Types correct, no duplicate PKs, strict null conversion
- **Gate C (Std → Enrich):** All CDS map to known dim, crosswalk coverage ≥98%
- **Gate D (Enrich → Final):** Policy rules pass, drift/outlier checks, lineage present
- **Gate E (Publish):** Serving contracts hold, backward compatibility tests pass

**Scale:**
- 1000+ districts, 10,000 schools, 60M+ rows
- 6 data sources: CDE Core, Assessments, District Docs, Product Evidence, Compliance, Market Signals
- 6 downstream applications: Attio CRM, Brevo Email, ML Framework, PD System, Vendor Intel, Readiness

**Idempotency:** All pipeline stages support replay from any checkpoint. Failed batches quarantine to `ops.quarantine` without blocking downstream.

**Tech Stack:** PostgreSQL (Supabase), Python (pandas, Great Expectations), Node.js, OpenTelemetry
**Location:** `docs/MASTERS/00-master-blueprint.md`, `docs/MASTERS/19a-pipeline-dag.md`

---

### 3. Context Management System

**Problem:** Exposing all 100+ tools to every Claude Code session risks accidental side-effects (e.g., junior agent sends email campaign). Need least-privilege access control without manual tool filtering.

**Solution:** Built 18 profile-scoped lazy-MCP proxies, each exposing a curated toolset:

**Profile Examples:**
- **`research`**: Google Drive/Docs/Gmail (read-only), Memory Service (search), Attio CRM (read-only)
- **`sales-draft`**: Email orchestration, email features, Attio intel (for draft generation)
- **`crm-write`**: Attio ops (40 tools), Attio orchestration (16 tools), requires G2 approval
- **`sales-send`**: Brevo (9 tools), email orchestrator (read-only), requires G3 approval (two-step commit)

**Approval Gates (G0-G3):**
- **G0:** Read-only, pure compute (no approval needed)
- **G1:** Internal writes (memory), draft generation (review required)
- **G2:** DB/CRM writes, remote execution (human approval required)
- **G3:** External send operations (email, campaigns) (two-step commit: preflight + explicit "SEND")

**Implementation:**
- Each proxy = separate `mcp-proxy` process with `config-{profile}.json`
- Hierarchy files (`hierarchy-{profile}/{server}/{tool}.json`) define exact tool allowlists
- Proxies spawn on Claude Code startup, expose 2 meta-tools each (`start_server`, `list_tools`)
- Actual tool schema loads lazily, keeping startup fast

**Token Optimization:**
- **Before:** 512k tokens (all 100+ tools loaded upfront)
- **After:** 74k tokens (18 proxies × 2 meta-tools = 36 tools at startup)
- **Result:** 87% reduction, enables larger context windows

**Tech Stack:** TypeScript, Model Context Protocol, shell scripts
**Location:** `.claude/lazy-mcp/config-*.json`, `.claude/lazy-mcp/hierarchy-*/`

---

## Technical Depth

### MCP Protocol & Tool Composition
- **Model Context Protocol (MCP):** Anthropic's standard for AI-tool interfaces
- **Server Architecture:** Each server exposes tools via JSON-RPC 2.0
- **Lazy Loading:** Tools load on first use, not startup (preserves token budget)
- **Composability:** Single tool can call multiple downstream MCP servers (e.g., `mcp-attio-orch` → `mcp-attio-ops` + `supabase_mcp`)

### Performance Optimization Patterns
- **ML Prefetching (Attio-Supabase):** Query pattern prediction reduces 70-90% of CRM read latency
- **Connection Pooling:** Singleton PostgreSQL pool with SSL support, prevents connection exhaustion
- **Materialized Views:** `ops.v_runs` aggregates event log for fast control plane queries
- **Index Strategy:** Composite indexes on (status, started_at DESC), (agent_name), (run_id)

### Idempotency & Retry Logic
- **Event Sourcing:** `ops.orchestrator_events` is append-only source of truth
- **Outbox Pattern:** Events batch-write to DB, dead-letter queue handles failures
- **Retry Configuration:** Max 2 requeue attempts, exponential backoff, dead-letter handler for poison messages
- **Dry-Run Support:** All G2+ operations support `dry_run:true` for preflight validation

### Infrastructure as Code
- **Database Migrations:** Versioned SQL files in `db/migrations/`, CI-enforced ordering
- **Schema Contracts:** JSON Schema + OpenAPI 3.1 drive validation (contract-first development)
- **RBAC:** Role-based access control with `noonchi_staff_rw` (write), `bi_reader` (read-only), app-specific roles
- **RLS Policies:** Row-level security on all org-grained tables (district, school, person)

---

## Diagram Preview

**3-Tier Pyramid:**
```
                    ┌─────────────────────────────┐
                    │   APPLICATIONS (BLUE)       │
                    │  CRAFT • Vendor Eval • PD   │
                    └─────────────────────────────┘
                    ┌─────────────────────────────┐
                    │   INTELLIGENCE (YELLOW)     │
                    │  Attio-Supabase • Enrich    │
                    └─────────────────────────────┘
    ┌───────────────────────────────────────────────────────┐
    │              FOUNDATION (GREEN)                       │
    │  100+ MCP tools • 20+ servers • 6 domains             │
    │  Sales • Data • Builder • Research • Memory • Routing │
    └───────────────────────────────────────────────────────┘
```

**Data Pipeline Flow:**
```
[6 Sources] → [6 Layers] → [6 Applications]
     ↓            ↓              ↓
  CDE Core      Raw         Attio CRM
  CAASPP        Stage       Brevo Email
  District      Std         ML Framework
  Product       Enrich      PD System
  Compliance    Semantic    Vendor Intel
  Market        Final       Readiness
     ↓            ↓              ↓
  [25 Quality Gates A-E]
```

*Full diagrams available in [3-tier-pyramid.png] and [data-pipeline-flow.png]*

---

## Related Pages

- **[Governance & Compliance](./GOVERNANCE.md)** - How infrastructure enforces FERPA/COPPA compliance
- **[Evaluation & Testing](./EVALS.md)** - How observability enables testing rigor

---

## Call-to-Action

**Hiring for infrastructure, distributed systems, or ML ops?**

The architecture diagrams only scratch the surface. Let's discuss scaling AI orchestration at your company.

**Contact:** [jason.lee.jaemin@gmail.com](mailto:jason.lee.jaemin@gmail.com) | [LinkedIn](https://linkedin.com/in/jason-jaemin-lee)

**Portfolio:** [jason-j-lee.com](https://jason-j-lee.com) - See all 25+ projects in interactive bento grid

---

## Code Samples

### MCP Server Example (Memory Service)

```python
# packages/mcp-memory-service-main/src/mcp_memory_service/web/api/mcp.py
@router.post("/api/v1/memories", status_code=201)
async def store_memory(request: MemoryStoreRequest):
    """Store memory with governance gates"""

    # P12 Write Gates (7 gates)
    gates_result = check_write_gates(request)
    if not gates_result.allowed:
        audit_log(event="WRITE_BLOCKED", reason=gates_result.reason)
        raise HTTPException(403, gates_result.reason)

    # PII Detection (K-12 specific)
    pii_scan = detect_pii(request.content)
    if pii_scan.has_critical:
        request.content = scrub_pii(request.content, pii_scan.matches)

    # Store with ABAC visibility
    memory_id = await storage.store(
        content=request.content,
        memory_type=request.memory_type,
        visibility=request.visibility  # band-based access control
    )

    # Audit Log (FERPA compliance)
    audit_log(
        event="MEMORY_STORED",
        actor=request.headers.get("X-Actor"),
        memory_id=memory_id,
        pii_scrubbed=pii_scan.has_critical
    )

    return {"memory_id": memory_id}
```

### Orchestrator CLI Example

```typescript
// packages/orchestrator/src/cli.ts
async function spawnAgent(agentName: string, task: string) {
  // Load agent spec from AGENT-REGISTRY
  const spec = await loadAgentSpec(agentName);

  // Check approval gate
  if (spec.gate >= 2) {
    console.log(`⚠️  Agent requires G${spec.gate} approval`);
    const approved = await promptApproval();
    if (!approved) return;
  }

  // Generate Task tool call
  const toolCall = {
    description: `${spec.role} - ${task.slice(0, 40)}...`,
    prompt: `${spec.prompt}\n\nTASK: ${task}`,
    subagent_type: spec.subagent_type,
    model: selectModel(spec.tier)  // Opus 4.5, Sonnet 4.5, Haiku
  };

  // Emit to event bus
  await eventBus.emit("agent.spawned", {
    agent_name: spec.name,
    tier: spec.tier,
    task_id: generateTaskId()
  });

  return toolCall;
}
```

### Data Pipeline Quality Gate

```sql
-- db/migrations/002_quality_gates.sql
-- Gate B: Stage → Std (Type Safety + Null Handling)

CREATE OR REPLACE FUNCTION std.strict_null(value TEXT)
RETURNS TEXT AS $$
BEGIN
  -- Only "", "*, "—" convert to NULL
  IF value IN ('', '*', '—') THEN
    RETURN NULL;
  END IF;

  -- All other pseudo-nulls quarantine
  IF value IN ('NA', 'N/A', '-', '.', 'n/a', 'NULL') THEN
    INSERT INTO ops.quarantine (
      layer, table_name, row_data, reason
    ) VALUES (
      'stage', TG_TABLE_NAME, ROW(NEW.*), 'pseudo_null_detected'
    );
    RETURN NULL;
  END IF;

  RETURN value;
END;
$$ LANGUAGE plpgsql;

-- Enforce on enrollment table
CREATE TRIGGER enforce_strict_null
  BEFORE INSERT OR UPDATE ON stage.fact_enrollment
  FOR EACH ROW
  EXECUTE FUNCTION std.strict_null(NEW.column_name);
```

---

**Built with:** TypeScript, Python, PostgreSQL, FastAPI, Node.js, Model Context Protocol (MCP), Claude Sonnet 4.5, OpenTelemetry

**Repository:** [00-ACTIVE-MCP-SYSTEM](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM) (private - available on request)
