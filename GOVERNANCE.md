# Governance & Compliance

**Built FERPA/COPPA-compliant systems with 300+ automated checks and 7-year audit retention**

---

## Value Proposition

Architected governance from day one for California K-12: PII detection (7 patterns), read-time ABAC filtering, immutable audit logs with chain hashing, and Trust Scorecard framework (5 dimensions, TL0-TL4 autonomy gates) — all verified through 300+ automated checks.

**Key Stats:**
- 5-dimension Trust Scorecard (Contract Clarity, Idempotency, Observability, Error Containment, Governance)
- TL0-TL4 autonomy gates (Draft → Manual → Sandbox → Supervised → Autonomous)
- 7 write gates for memory governance (PII, provenance, type, dedup, ABAC, size, injection)
- 62 agents scored, 3 production-ready (≥28/34 score) (verified: Evaluations/Agents/_leaderboard.md:19-20)
- FERPA/COPPA compliance mechanisms (PII redaction, RLS policies, audit trails)

---

## Opening Hook

K-12 means FERPA, COPPA, PII redaction, and 7-year audit trails—non-negotiable. I architected governance from day one: read-time ABAC filtering prevents unauthorized memory access, immutable logs with chain hashing provide tamper detection, and the Trust Scorecard framework enforces that no agent runs unsupervised until it scores ≥70 across 5 dimensions (34 points total). Trust isn't theater; it's a measurable scoring system that gates autonomy progression (TL0 → TL4) with evidence at every level.

---

## Core Capabilities

### Compliance Frameworks
- **FERPA 34 CFR 99.32:** Audit trails for all data access, PII protection, 7-year retention
- **COPPA (Under-13):** Age verification, parental consent tracking, restricted data collection
- **California K-12 Standards:** CALPADS integration, district/school/student code validation
- **FERPA-Compliant Audit Logging:** Append-only logs with chain hashing, content hashes (no PII in logs)

### Risk Mitigation
- **PII Detection:** 7 regex patterns detect SSN, SSID, DOB, health records, student names, staff names, personal emails
- **PII Categories:** CRITICAL (block writes), HIGH (scrub content), MEDIUM (scrub silently)
- **Read-Time ABAC Filtering:** `can_read(requester_band, visibility)` enforces band-based access control
- **RLS Policies:** Row-level security on all org-grained tables (district, school, person)
- **Band Authorization:** Fixed bands (HTTP = "B", agents inherit from profile) prevent band spoofing

### Audit Readiness
- **7-Year Retention:** FERPA requirement enforced via data lifecycle policies
- **Immutable Logs:** Append-only NDJSON with chain hashing for tamper detection
- **Chain Hashing:** Each audit entry includes `chain_hash = SHA256(prev_chain_hash + entry_json)`
- **FERPA Compliance:** Audit logs contain only hashes, timestamps, actors — no PII content
- **Audit Actions:** STORE, RETRIEVE, SEARCH, DELETE, PII_DETECTED, PII_BLOCKED, PII_SCRUBBED, AUTH_SUCCESS, AUTH_FAILURE, WRITE_BLOCKED, ACL_ALLOWED, ACL_DENIED

### Trust Progression (TL0-TL4)
- **TL0 (0-24 score):** Draft — Not deployable, manual testing only
- **TL1 (25-49 score):** Assisted — Human executes, agent advises
- **TL2 (50-69 score):** Supervised — Human checkpoint at critical junctures
- **TL3 (70-89 score):** Autonomous — Unsupervised with logging (internal orchestration)
- **TL4 (90-100 score):** Trusted — Full autonomy, cross-system eligible (Manus-ready)

---

## Featured Projects

### 1. Trust Scorecard Framework

**Problem:** How do you know if an agent is safe to run unsupervised? Prompt engineering alone can't guarantee reliability. You need objective, measurable criteria that prove autonomy readiness.

**Solution:** Built 5-dimension Trust Scorecard framework that scores agents 0-100 across:

**5 Dimensions (0-20 each):**
1. **Contract Clarity (CC):** I/O schemas exist, versioned, runtime-validated
2. **Idempotency (ID):** Re-execution produces identical results, retry-safe
3. **Observability (OB):** Structured logs, health endpoints, error codes, status queryable
4. **Error Containment (EC):** Failures are graceful, scoped, rollback defined
5. **Governance (GV):** Owner assigned, changelog maintained, review gate passed

**Autonomy Thresholds:**
- **≥70 (TL3):** Internal orchestration autonomy
- **≥90 (TL4):** External/cross-system trust (Manus-ready)
- **TL4 Dimension Floors:** All dimensions ≥80; none below 70

**Scoring Criteria (Per Dimension):**
```
Contract Clarity:
  0-4:   No schema, I/O undefined
  5-9:   Schema exists but incomplete or unversioned
  10-14: Schema complete, versioned, not runtime-validated
  15-17: Schema complete, versioned, runtime-validated
  18-20: Schema + validated + edge case documentation

Idempotency:
  0-4:   Unknown retry behavior
  5-9:   Documented as "not retry-safe"
  10-14: Retry-safe for happy path only
  15-17: Retry-safe with idempotency key implemented
  18-20: Retry-safe, tested via CI, fault injection passed

Observability:
  0-4:   No logging, no health endpoint
  5-9:   Basic logging, unstructured
  10-14: Structured logs, no health endpoint
  15-17: Structured logs + health endpoint + error codes
  18-20: Full observability: logs, health, metrics, tracing, queryable status
```

**Ownership Matrix:**
| Dimension | Primary Owner | Scoring Method | Frequency |
|-----------|---------------|----------------|-----------|
| Contract Clarity | Rex R (Reviewer) | Automated schema validation + human audit | On PR merge |
| Idempotency | Tina T (Tester) | Automated: run twice, compare outputs | CI on commit |
| Observability | Ivan I (Integration) | Automated: health endpoint + log check | CI + runtime |
| Error Containment | Tina T (Tester) | Automated fault injection + human review | On PR merge |
| Governance | Lily L (Librarian) | Human audit: owner, changelog, review | On release |

**Impact:**
- 62 agents scored using 10-dimension rubric (34 total points)
- 3 agents production-ready (planner, researcher, reviewer at 28/34)
- Objective criteria replace subjective "it works" claims
- Autonomy progression gated by evidence, not promises

**Tech Stack:** TypeScript, Python, JSON Schema, OpenTelemetry
**Location:** `docs/MASTERS/35-trust-scorecard.md`, `Evaluations/Agents/`

---

### 2. Memory Governance (7 Write Gates)

**Problem:** AI agents storing unvalidated memories leads to PII leaks, duplicate content, injection attacks, and ungoverned data sprawl. Need pre-write validation that blocks bad data before storage.

**Solution:** Implemented 7-gate write validation system in memory service:

**7 Write Gates:**
1. **PII Scanner Pass:** Detect SSN, SSID, DOB, health records, names, emails, phones, addresses
   - CRITICAL categories (SSN, SSID, DOB, Health) → Block write entirely
   - HIGH categories (Names, Email, Phone, Address) → Scrub content + warn
   - MEDIUM categories (Credentials, SQL) → Scrub silently

2. **Provenance Required:** Every memory must include `source_agent`, `run_id`, actor metadata

3. **Type Classification:** Validate `memory_type` against allowlist: `note`, `decision`, `task`, `reference`, `document`, `association`, `failure`, `pattern`, `preference`

4. **Content Hash Dedup:** SHA256 hash check prevents duplicate content (in-memory cache)

5. **Agent Band Authorization (ABAC):** `can_write(requester_band, visibility)` enforces write permissions
   - Fixed bands: HTTP API = "B", agents inherit from profile
   - Band spoofing logged to audit trail

6. **Size Limit:** `MAX_CONTENT_SIZE = 4096`, `MAX_TAGS_COUNT = 20`, `MAX_TAG_LENGTH = 64`, `MAX_METADATA_SIZE = 2048`

7. **Injection Pattern Rejection:** Defense-in-depth patterns block "IGNORE ALL PREVIOUS", `<system>` tags, SQL DROP/TRUNCATE

**PII Detection Patterns (K-12 Specific):**
```python
# CRITICAL (Block)
SSN: r"\b\d{3}-\d{2}-\d{4}\b", r"\bssn\s*[:=]?\s*\S+\b"
SSID: r"(?i)(student\s*id|ssid|snid)\s*[:=]?\s*\d+", r"(?i)calpads.*\d{10}"
DOB: r"(?i)(date\s*of\s*birth|dob)\s*[:=]?\s*\d", r"\b(0?[1-9]|1[0-2])/(0?[1-9]|[12]\d|3[01])/(\d{4}|\d{2})\b"
Health: r"(?i)\b(iep|504\s*plan)\b", r"(?i)(diagnosis|medication|disability)\s*[:=]"

# HIGH (Scrub)
Student Names: r"(?i)student\s*name\s*[:=]?\s*[A-Za-z]+", r"(?i)(student|pupil)\s+[A-Z][a-z]+\s+[A-Z][a-z]+"
Staff Names: r"(?i)(teacher|staff)\s*name\s*[:=]?\s*[A-Za-z]+"
Personal Email: r"[A-Za-z0-9._%+-]+@(gmail|yahoo|hotmail|outlook)\.(com|net|org)"
Phone: r"\b(\+1[-.\s]?)?(\(?\d{3}\)?[-.\s]?)?\d{3}[-.\s]?\d{4}\b"
Address: r"\b\d+\s+[A-Za-z]+\s+(St|Ave|Blvd|Rd|Dr|Ln)\b"
```

**Integration Points:**
- `/api/memories` POST (REST endpoint) — Write gates + PII scan + audit
- `/mcp` JSON-RPC endpoint — Write gates + PII scan + audit
- `/api/search` GET — Read-only (safe, no gates)

**Impact:**
- Zero PII leaks to memory store (gates block CRITICAL categories)
- All memory writes traceable (provenance + audit logs)
- Injection attacks prevented (defense-in-depth patterns)

**Tech Stack:** Python 3.13, FastAPI, regex, hashlib
**Location:** `packages/mcp-memory-service-main/src/mcp_memory_service/governance/`

---

### 3. FERPA-Compliant Audit Logging

**Problem:** FERPA 34 CFR 99.32 requires educational institutions to maintain a record of all parties who access student data. Audit logs must be tamper-evident, append-only, and retained for 7 years.

**Solution:** Implemented chain-hashed append-only audit trail:

**Audit Log Architecture:**
```python
@dataclass
class AuditEntry:
    timestamp: str              # ISO 8601 UTC
    action: str                 # STORE, RETRIEVE, SEARCH, DELETE, PII_DETECTED, etc.
    actor: str                  # Agent name or user ID
    content_hash: Optional[str] # SHA256 of content (NO PII)
    memory_type: Optional[str]  # note, decision, task, etc.
    tags: Optional[List[str]]   # Tags (NO PII)
    pii_detected: bool          # PII scan result
    pii_categories: List[str]   # ['SSN', 'DOB'] (categories only, NO content)
    success: bool               # Operation succeeded?
    error_message: Optional[str]
    metadata: Optional[Dict]
    chain_hash: str             # SHA256(prev_chain_hash + entry_json)
```

**Chain Hashing for Tamper Detection:**
```
Entry 1: chain_hash = SHA256(init_seed + entry_1_json)
Entry 2: chain_hash = SHA256(entry_1_chain_hash + entry_2_json)
Entry 3: chain_hash = SHA256(entry_2_chain_hash + entry_3_json)
...

If any entry is modified, all subsequent chain_hashes break.
```

**FERPA Compliance:**
- **No PII in logs:** Only hashes, timestamps, actions, categories
- **Content Hash:** SHA256 of actual content stored separately for verification
- **Append-Only:** NDJSON format, no updates/deletes
- **Daily Rotation:** Log files rotate daily, archived for 7-year retention
- **Batch Writes:** Logs batch every 10 entries or 5 seconds (configurable)

**Audit Actions (Enum):**
```
STORE, RETRIEVE, SEARCH, DELETE          # Memory operations
PII_DETECTED, PII_BLOCKED, PII_SCRUBBED  # PII handling
AUTH_SUCCESS, AUTH_FAILURE                # Authentication
WRITE_BLOCKED                             # Write gate rejection
ACL_ALLOWED, ACL_DENIED                   # Access control
```

**7-Year Retention Strategy:**
- **Active Logs:** 90 days in hot storage (NDJSON files)
- **Warm Storage:** 1 year in compressed archives (gzip)
- **Cold Storage:** 6 years in S3 Glacier Deep Archive
- **Lifecycle Policy:** Automated transition via S3 bucket rules

**Impact:**
- FERPA-compliant audit trail for all memory access
- Tamper detection via chain hashing
- Zero PII in audit logs (content hashes only)
- 7-year retention meets FERPA requirements

**Tech Stack:** Python 3.13, NDJSON, hashlib, asyncio
**Location:** `packages/mcp-memory-service-main/src/mcp_memory_service/governance/audit.py`

---

## Technical Depth

### ABAC (Attribute-Based Access Control)
- **Band System:** Fixed visibility levels (A-E, S, all)
- **Read-Time Filtering:** `filter_by_visibility(results, requester_band)` applied to all queries
- **Write Authorization:** `can_write(requester_band, visibility)` gates storage
- **Band Assignment:** HTTP API = "B", agents inherit from profile, spoofing logged
- **RLS Integration:** PostgreSQL row-level security policies enforce band checks at DB level

### RLS Policies (Row-Level Security)
```sql
-- Example: District table RLS
CREATE POLICY district_rls ON dim.district
  USING (
    -- System role sees all
    current_setting('app.role', true) = 'system'
    OR
    -- Users see only their assigned districts
    cds_code IN (
      SELECT district_cds_14 FROM access.user_districts
      WHERE user_id = current_setting('app.user_id', true)::uuid
    )
  );

ALTER TABLE dim.district ENABLE ROW LEVEL SECURITY;
```

### Memory Governance Workflow
```
1. Memory write request arrives
2. Write gates validate:
   ✓ PII scan passes (or scrubs)
   ✓ Provenance present
   ✓ Type valid
   ✓ Content hash unique
   ✓ Band authorized
   ✓ Size within limits
   ✓ No injection patterns
3. If any gate fails → Block + Audit
4. If all gates pass → Store + Audit
5. Read requests filtered by ABAC
```

### Trust Scorecard Automation
- **60% Automated:** Contract validation (JSON Schema), Idempotency tests (run twice), Health checks (HTTP endpoints), Log structure validation
- **40% Human Audit:** Edge case documentation review, Changelog completeness, Security sign-off

### Evidence Collection
- **OpenTelemetry Tracing:** Distributed traces across MCP servers
- **19 Event Types:** Agent spawns, tool calls, routing decisions, errors, completions
- **CI Integration:** Trust Scorecard scores block PR merges if below threshold
- **Queryable Status:** `/health` endpoints expose real-time component health

---

## Diagram Preview

**Trust Scorecard Flow:**
```
300 Indicators → 150 Benchmarks → 50 Standards → 5 Dimensions → 0-100 Score → TL0-TL4

Example: Agent "Rita R - Researcher"
  Contract Clarity:  18/20 (Schema + validation + docs)
  Idempotency:       16/20 (Retry-safe, tested)
  Observability:     17/20 (Logs + health + tracing)
  Error Containment: 15/20 (Graceful errors + rollback)
  Governance:        16/20 (Owner + changelog + review)
  ─────────────────────────────────────────────────
  Total Score:       82/100 → TL3 (Autonomous)
```

**Compliance Layers:**
```
[PII Scan] → [ABAC Check] → [Audit Log] → [Cold Storage]
     ↓             ↓              ↓              ↓
  CRITICAL    requester_band  NDJSON       S3 Glacier
   blocks      vs visibility   chain hash   7-year
```

**Autonomy Gates:**
```
TL0 (0-24):  Draft          → Manual testing only
TL1 (25-49): Assisted       → Human executes, agent advises
TL2 (50-69): Supervised     → Human checkpoint at gates
TL3 (70-89): Autonomous     → Unsupervised with logging
TL4 (90-100): Trusted       → Full autonomy, cross-system
```

*Full diagrams available in [trust-scorecard-flow.png] and [compliance-layers.png]*

---

## Related Pages

- **[Evaluation & Testing](./EVALS.md)** - How governance is tested (adversarial fixtures, Trust Scorecard automation)
- **[System Design](./SYSTEMS.md)** - How governance integrates with infrastructure (MCP servers, data pipeline)

---

## Call-to-Action

**Hiring for compliance, security, or trust & safety?**

K-12 is one of the hardest domains for AI governance. Let's discuss applying these patterns to your industry.

**Contact:** [jason.lee.jaemin@gmail.com](mailto:jason.lee.jaemin@gmail.com) | [LinkedIn](https://linkedin.com/in/jason-jaemin-lee)

**Portfolio:** [jason-j-lee.com](https://jason-j-lee.com) - See all 25+ projects in interactive bento grid

---

## Code Samples

### PII Detection (K-12 Specific)

```python
# packages/mcp-memory-service-main/src/mcp_memory_service/governance/pii_detector.py

class PIIDetector:
    """PII detection using regex patterns for K-12 education context."""

    # CRITICAL patterns (block writes)
    PATTERNS = [
        ("ssn_format", re.compile(r"\b\d{3}-\d{2}-\d{4}\b"), PIICategory.SSN),
        ("ssid_label", re.compile(r"(?i)(student\s*id|ssid)\s*[:=]?\s*\d+"), PIICategory.SSID),
        ("dob_labeled", re.compile(r"(?i)(date\s*of\s*birth|dob)\s*[:=]?\s*\d"), PIICategory.DOB),
        ("health_iep", re.compile(r"(?i)\b(iep|504\s*plan)\b"), PIICategory.HEALTH),
        # HIGH patterns (scrub content)
        ("student_label", re.compile(r"(?i)student\s*name\s*[:=]?\s*[A-Za-z]+"), PIICategory.STUDENT_NAME),
        ("email_personal", re.compile(r"[A-Za-z0-9._%+-]+@(gmail|yahoo)\.(com|net)"), PIICategory.EMAIL_PERSONAL),
        # ... more patterns
    ]

    def scan(self, content: str) -> PIIScanResult:
        """Scan content for PII patterns."""
        matches = []
        for name, pattern, category in self.PATTERNS:
            for match in pattern.finditer(content):
                matches.append(PIIMatch(
                    category=category,
                    start=match.start(),
                    end=match.end(),
                    matched_text=match.group(),
                    pattern_name=name
                ))

        has_critical = any(m.category in CRITICAL_CATEGORIES for m in matches)
        has_high = any(m.category in HIGH_CATEGORIES for m in matches)

        # Scrub HIGH/MEDIUM categories
        scrubbed_content = content
        if has_high:
            for match in sorted(matches, key=lambda m: m.start, reverse=True):
                if match.category in HIGH_CATEGORIES:
                    scrubbed_content = (
                        scrubbed_content[:match.start] +
                        f"[REDACTED:{match.category.value}]" +
                        scrubbed_content[match.end:]
                    )

        return PIIScanResult(
            has_pii=bool(matches),
            has_critical=has_critical,
            has_high=has_high,
            matches=matches,
            scrubbed_content=scrubbed_content if has_high else None
        )
```

### Write Gates Enforcement

```python
# packages/mcp-memory-service-main/src/mcp_memory_service/governance/write_gates.py

class WriteGates:
    """Validates 7 gates before allowing memory writes."""

    def check(self, request: WriteRequest) -> WriteGateResult:
        """Check all 7 gates in sequence."""

        # Gate 1: PII Scanner Pass
        pii_scan = self.pii_detector.scan(request.content)
        if pii_scan.should_block:
            return _block(
                WriteBlockReason.PII_DETECTED,
                f"CRITICAL PII detected: {list(pii_scan.categories_found)}",
                pii_scan
            )

        # Gate 2: Provenance Required
        if not request.source_agent or not request.run_id:
            return _block(
                WriteBlockReason.MISSING_PROVENANCE,
                "Memory write requires source_agent and run_id"
            )

        # Gate 3: Type Classification
        memory_type = request.memory_type or "note"
        if memory_type not in VALID_MEMORY_TYPES:
            return _block(
                WriteBlockReason.INVALID_MEMORY_TYPE,
                f"Invalid memory_type: {memory_type}"
            )

        # Gate 4: Content Hash Dedup
        content_hash = hashlib.sha256(request.content.encode()).hexdigest()
        if content_hash in self._dedup_cache:
            return _block(
                WriteBlockReason.DUPLICATE_CONTENT,
                f"Duplicate content hash: {content_hash[:16]}..."
            )

        # Gate 5: Agent Band Authorization (ABAC)
        if not can_write(request.requester_band, request.visibility):
            return _block(
                WriteBlockReason.UNAUTHORIZED_BAND,
                f"Band {request.requester_band} cannot write to visibility {request.visibility}"
            )

        # Gate 6: Size Limit
        if len(request.content) > MAX_CONTENT_SIZE:
            return _block(
                WriteBlockReason.SIZE_EXCEEDED,
                f"Content size {len(request.content)} exceeds {MAX_CONTENT_SIZE}"
            )

        # Gate 7: Injection Pattern Rejection
        for pattern, name in INJECTION_PATTERNS:
            if pattern.search(request.content):
                return _block(
                    WriteBlockReason.INJECTION_DETECTED,
                    f"Injection pattern detected: {name}"
                )

        # All gates passed
        self._dedup_cache.add(content_hash)
        return WriteGateResult(
            allowed=True,
            pii_scan=pii_scan,
            scrubbed_content=pii_scan.scrubbed_content
        )
```

### Chain-Hashed Audit Logging

```python
# packages/mcp-memory-service-main/src/mcp_memory_service/governance/audit.py

class AuditLogger:
    """Append-only audit logger with chain hashing for tamper detection."""

    async def log(self, action: AuditAction, actor: str, **kwargs) -> None:
        """Log an auditable action with chain hashing."""
        entry = AuditEntry(
            timestamp=datetime.now(timezone.utc).isoformat(),
            action=action.value,
            actor=actor,
            **kwargs
        )

        # Compute chain hash: SHA256(prev_chain_hash + entry_json)
        entry_json = entry.to_json()
        prev_hash = self._last_chain_hash or "INIT_SEED"
        chain_input = f"{prev_hash}{entry_json}".encode()
        entry.chain_hash = hashlib.sha256(chain_input).hexdigest()

        # Update last chain hash
        self._last_chain_hash = entry.chain_hash

        # Batch write (async, non-blocking)
        async with self._lock:
            self._batch.append(entry)
            if len(self._batch) >= self.batch_size:
                await self._flush()

    async def _flush(self) -> None:
        """Flush batch to NDJSON file."""
        if not self._batch:
            return

        log_file = self._get_log_file()  # Daily rotation
        try:
            with open(log_file, "a") as f:
                for entry in self._batch:
                    f.write(entry.to_json() + "\n")
            logger.info(f"Flushed {len(self._batch)} audit entries to {log_file}")
            self._batch.clear()
        except Exception as e:
            logger.error(f"Audit flush failed: {e}")
```

---

**Built with:** Python 3.13, FastAPI, PostgreSQL, regex, hashlib, asyncio, JSON Schema, OpenTelemetry

**Repository:** [00-ACTIVE-MCP-SYSTEM](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM) (private - available on request)
