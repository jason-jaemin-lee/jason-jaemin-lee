# AI Product Design

**Designed 9 agent products that preserve authenticity while building competence**

---

## Value Proposition

Built 9 domain-specific agent products for K-12 educators that solve real instructional problems without stripping student voice or ignoring cultural assets. From adaptive learning (Spiraly) to voice-protective writing feedback (Langston AI), each agent prioritizes human agency over automation, authenticity over efficiency.

**Key Stats:**
- 9 distinct agent products for K-12 users (Spiraly, Norma, Langston AI, Soto, Delineate, Abacus, Reclass, Exemplarable, Aidstep)
- Voice-protective rubrics (Langston AI) - verified: [github-profile-draft/README.md:21](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM/blob/develop/github-profile-draft/README.md#L21)
- Zero-hallucination rule (Exemplarable)
- Agency-first design (Soto ELL tools)

---

## Opening Hook

Most AI education tools strip student voice or ignore cultural assets. I designed 9 agents that preserve authenticity while building competence. Each agent solves a specific K-12 pain point with domain-specific UX: adaptive learning that respects learning differences (Spiraly), writing feedback that protects student voice (Langston AI), and ELL tools that honor the Silent Period (Soto). The result: AI that empowers teachers and students, not replaces them.

---

## Core Capabilities

### User Empathy & K-12 Personas
- **Teacher workflows:** Designed around grading, feedback, differentiation, and data interpretation
- **Student agency:** Features that let students respond, negotiate, and maintain voice
- **Counselor collaboration:** Tools that preserve student privacy while surfacing insights
- **ELL-specific patterns:** Silent Period protocols, gesture-first feedback, code-switching support

### Design Decisions (Not Just Features)
- **Voice-protective rubrics:** Langston AI feedback focuses on craft choices, not correctness alone
- **Cultural asset mapping:** Exemplarable essay examples include Black, Latino, and Asian-American voices
- **Mission Control patterns:** Scenario cards let teachers preview agent behavior before deployment
- **Drift feedback loops:** Norma monitors when rubrics drift from initial calibration

### Product Features
- **Scenario cards:** Educators see "what if" examples before using agents in real classrooms
- **Drift detection:** Automated rater consistency checks (Norma)
- **Semantic alignment:** NGSS tagging with cross-subject bridges (Delineate)
- **Explainability:** Every agent recommendation includes reasoning, not just scores

### Cultural Sensitivity & Ethics
- **Black/Latino student voice preservation:** Feature not an afterthought
- **Disability-first accessibility:** IEP compatibility, adaptive interfaces
- **Equity-aware defaults:** Avoid perpetuating achievement gaps through design choices

---

## Featured Projects

### 1. Langston AI (Voice-Protective Writing Feedback)

**Problem:** Most writing feedback tools focus on grammar and mechanics, erasing the student's authentic voice. Students get the message: "write like adults want you to write," not "develop your distinctive voice."

**Solution:** Built voice-protective writing feedback agent that:
- **Separates craft from correctness:** Feedback focuses on rhetorical choices (voice, audience, structure), not errors
- **Uses culturally-affirming rubrics:** Examples showcase Black, Latino, Asian-American student writers
- **Preserves student choice:** Suggestions are options, not imperatives
- **Explainability:** Every comment includes reasoning tied to pedagogical framework

**Architecture:**
```
Student writes → Langston AI analyzes → Voice-protective rubric applied
   ↓
Feedback generated:
  ✓ "You shifted audience mid-essay (line 12). Intentional? Could strengthen by..."
  ✗ NOT: "Run-on sentence, fix this"
   ↓
Teacher reviews → Approves/modifies → Delivers to student with context
```

**Key Features:**
- **Craft analysis:** Detects voice markers (repetition, dialect, code-switching, emphasis)
- **Cultural referents:** 50+ example texts from diverse authors
- **Revision pathway:** Suggests 3-5 revision options, not prescriptive fixes
- **Teacher control:** Rubric customizable per assignment

**Impact:**
- Preserves student voice while building writing competence
- Reduces time on mechanical feedback for teachers
- Encourages linguistic plurality (code-switching seen as strength)

**Tech Stack:** Claude (analysis), PostgreSQL (rubric engine), React (teacher UI)
**Location:** Framework in [.session_notes/GITHUB-PROFILE-5-PAGE-FRAMEWORK.md:119](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM/blob/develop/.session_notes/GITHUB-PROFILE-5-PAGE-FRAMEWORK.md#L119)

---

### 2. Spiraly (Adaptive Learning with Knowledge Tracing + DRL)

**Problem:** Most adaptive learning systems treat students as data points. After a wrong answer, the system says "you're not ready for this" or "you got lucky." No learning theory, no agency.

**Solution:** Built hybrid scheduling engine combining:
- **Knowledge Tracing (simpleKT):** Transformer-based model for probabilistic mastery estimation
- **Deep Reinforcement Learning (DRL-SRS):** Policy gradient optimization to maximize learning gain per session
- **Student agency:** 3 scheduling modes (System Suggested, Student Choice, Hybrid)

**Architecture:**
```
Student performance history → simpleKT mastery estimation → Item difficulty/discrimination
   ↓
Reinforcement learning policy: What item sequence maximizes expected learning?
   ↓
Student sees:
  🤖 "System suggests this (because you've mastered X, struggling with Y)"
  👤 "Pick your own path"
  ⚖️ "Balanced" (system + student input)
   ↓
Item delivered → Student responds → Updated knowledge state → Loop
```

**Key Features:**
- **Hybrid simpleKT+DRL scheduler:** Transformer-based knowledge tracing + policy gradient learning
- **Learning gain optimization:** Maximizes expected learning per session (not just engagement)
- **Student choice modes:** Agency as feature, not afterthought
- **Feedback quality:** Immediate post-response feedback calibrated to difficulty

**Research Grounding:**
- IRT from psychometrics literature (Lord & Novick, 1968)
- DRL from educational psychology (Bloom's 2-sigma problem)
- Student agency from self-determination theory (Deci & Ryan, 2000)

**Impact:**
- Adaptive without removing student voice
- Data-driven scheduling grounded in learning science
- Extensible to any subject (math, ELA, science)

**Tech Stack:** Python (DRL policy), scikit-learn (IRT calibration), TypeScript (scheduler UI)
**Location:** Portfolio reference at [github-profile-draft/README.md:22](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM/blob/develop/github-profile-draft/README.md#L22)

---

### 3. Soto (ELL Silent Period Tools & Agency-First Design)

**Problem:** ELL students in the "Silent Period" (weeks 1-6) need comprehensible input, not forced production. Most tools push speaking before students are ready. Teachers lack instruments to assess comprehension during silent period.

**Solution:** Built comprehension-first toolkit that honors language acquisition stages:
- **Gesture-first feedback:** Students respond with gestures, drawings, or L1 before speaking
- **Comprehensible input library:** Scaffolded scenario cards with transcript support
- **Silent Period assessment:** Formal tools to evaluate comprehension (not speaking) during early stages
- **Teacher coaching:** Rubrics for gesture interpretation, questioning techniques

**Architecture:**
```
Student in Silent Period (Weeks 1-6)
   ↓
Sees comprehensible input (video + visuals + optional transcript in L1)
   ↓
Responds via:
  🤐 Gesture (nod/shake head/point)
  🎨 Drawing (label diagram, choose image)
  📝 L1 text (brief response in home language)
   ↓
Teacher sees: "Student understood concept Y (evidence: gestured correctly to 3/4 comprehension checks)"
   ↓
Cumulative evidence → When student ready to speak, teacher has confidence they'll succeed
```

**Key Features:**
- **Gesture response protocol:** Formal rubric for interpreting (and validating) gesture-based responses
- **L1 integration:** Code-switching encouraged (L1 shows comprehension, not deficiency)
- **Comprehensible input library:** Curated scenario cards for essential needs (bathroom, water, help, sick)
- **Teacher coaching:** Silent Period timeline, questioning techniques, common misconceptions

**Research Grounding:**
- Second Language Acquisition theory (Krashen, 1985)
- Comprehensible input hypothesis (i+1)
- Gesture as language data (McNeill, 2005)

**Impact:**
- Honors language acquisition stages
- Reduces pressure on ELL students
- Teachers get evidence for instructional decisions
- Reduces misidentification of ELL as learning-disabled

**Tech Stack:** React (input library), Python (response classification), PostgreSQL (evidence tracking)
**Location:** Framework reference at [.session_notes/GITHUB-PROFILE-5-PAGE-FRAMEWORK.md:120](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM/blob/develop/.session_notes/GITHUB-PROFILE-5-PAGE-FRAMEWORK.md#L120)

---

## Core Agents at a Glance

**The 9 Agent Products:**

| Agent | Core Purpose | User | Design Principle |
|-------|--------------|------|-------------------|
| **Spiraly** | Adaptive learning scheduler (simpleKT+DRL) | Students + Teachers | Agency-first (3 modes) |
| **Norma** | Rubric calibration & rater drift detection | Teachers | Data-informed collaboration |
| **Langston AI** | Voice-protective writing feedback | Students + Teachers | Preserve authenticity |
| **Soto** | ELL Silent Period tools & assessment | ELL Students + Teachers | Comprehensible input first |
| **Delineate** | NGSS alignment & 3-dimension tagging | Teachers | Semantic clarity |
| **Abacus** | Math curriculum coherence mapping | Teachers + Curriculum Leaders | Coherent scope sequence |
| **Reclass** | EL reclassification process (role-gated) | Counselors + Principals | Evidence-based gates |
| **Exemplarable** | Rubric-grounded feedback (anti-hallucination constraint) | Teachers + Students | Grounded in examples |
| **Aidstep** | FAFSA/CADAA tracking & deadline awareness | Students + Counselors | Remove friction |

---

## Technical Depth

### Design Patterns for Ethical AI

**Voice Preservation (Langston AI Pattern):**
- Separate **craft analysis** (what rhetorical choices did the student make?) from **correctness** (grammar/mechanics)
- Feedback grounded in **pedagogical frameworks**, not just pattern-matching
- **Examples > Rules:** Show 3-5 alternatives instead of prescriptive fixes
- **Student choice loop:** Suggestions are options, student decides what to revise

**Agency First (Spiraly & Soto Pattern):**
- Multiple scheduling/interaction modes (System-driven, Student-driven, Hybrid)
- Transparent **reasoning:** "I suggested this because..."
- **Teachable feedback:** Explain why item was chosen, what misconception it addresses
- **Cumulative evidence:** Track learning gains across multiple attempts, surface patterns to teacher

**Zero-Hallucination (Exemplarable Pattern):**
- **Grounded feedback only:** Every example/suggestion linked to actual student essays (from corpus)
- **No synthetic examples:** Avoid generating fake "student work" that misleads
- **Audit trail:** Teachers can click through to original example that inspired feedback
- **Rule-based generation:** Feedback constructed from vetted templates, not LLM-generated prose

### Data Structure Patterns

**Rubric Engine (Norma, Langston AI):**
```typescript
interface Rubric {
  id: string;
  name: string;
  dimensions: {
    name: string;                    // "Voice", "Organization", etc.
    exemplars: StudentExample[];     // Real examples from corpus
    rubricLevels: {
      score: number;                 // 4-1
      descriptor: string;            // "Student shows strong command of..."
      exemplars: StudentExample[];   // Examples at this level
    }[];
  }[];
  calibrationHistory: {
    date: string;
    agreeance: number;              // Inter-rater reliability (Cohen's kappa)
    driftDetected: boolean;          // Has rubric interpretation drifted?
    alignmentCheck: number;          // Matches original rubric intent? (0-1)
  }[];
}
```

**Evidence Accumulation (Soto, Spiraly):**
```typescript
interface LearningEvidence {
  studentId: string;
  conceptId: string;
  evidence: {
    type: "response" | "gesture" | "drawing" | "l1_text";  // Response modality
    timestamp: ISO8601;
    correct: boolean;
    confidence: number;              // 0.0-1.0 (How clear was the signal?)
    context: string;                 // "Silent Period Week 2"
  }[];
  summary: {
    masteredAt?: ISO8601;            // When did evidence accumulate to 85%+ correct?
    daysToMastery: number;           // From first encounter to 85%+
    modalities: string[];            // Which response types worked best?
  };
}
```

### Evaluation Metrics

**Voice Quality (Langston AI):**
- **Authenticity score:** Does feedback preserve student's linguistic choices?
- **Agency score:** % of suggestions student adopts (not prescribed)
- **Discourse growth:** Pre/post analysis of student writing across semester

**Learning Gain (Spiraly):**
- **Normalized learning gain:** (Post - Pre) / (Max - Pre) — controls for ceiling effects
- **Scheduling efficiency:** Learning gain per minute of interaction
- **Student preference:** Which mode (System/Student/Hybrid) led to better outcomes?

**Equity (All Agents):**
- **Gap analysis:** Does agent reduce or perpetuate achievement gaps?
- **Representation:** Are diverse examples, students, voices visible in feedback?
- **Access:** Can all students (including those with disabilities) use all features?

---

## Product Gallery & Diagrams

### Agent Product Grid

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      9 K-12 AGENT PRODUCTS                                   │
├──────────────┬──────────────┬──────────────┬──────────────┬──────────────────┤
│              │              │              │              │                  │
│   Spiraly    │   Norma      │ Langston AI  │    Soto      │  Delineate       │
│  Adaptive    │  Calibration │   Voice      │     ELL      │   NGSS           │
│  Learning    │   & Drift    │ Protective   │  Silent      │  Alignment       │
│              │   Detection  │   Feedback   │   Period     │                  │
│              │              │              │              │                  │
├──────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│              │              │              │              │                  │
│   Abacus     │   Reclass    │Exemplarable  │   Aidstep    │    +             │
│   Math       │      EL      │   Rubric     │    FAFSA     │  Professional    │
│ Curriculum   │ Reclassif.   │  Grounded    │   CADAA      │   Development    │
│ Coherence    │  Process     │  Feedback    │  Tracking    │   & AI Readiness │
│  Mapping     │  (Role-Gated)│             │              │                  │
│              │              │              │              │                  │
└──────────────┴──────────────┴──────────────┴──────────────┴──────────────────┘

Core Design Principles Across All 9:
  ✓ Agency-First: Student/teacher choice, not just automation
  ✓ Voice-Preserving: Authenticity + competence, not either/or
  ✓ Evidence-Grounded: Decisions traceable to data + pedagogy
  ✓ Culturally-Aware: Examples & feedback reflect diverse students
```

### User Journey Map (Langston AI Example)

```
STUDENT PATH:
  1. Writes essay (first draft)
     ↓
  2. Submits to Langston AI
     ↓
  3. Sees voice-protective feedback
     - "You shifted audience mid-essay (line 12). Intentional?"
     - "3 revision options: [Option A] [Option B] [Option C]"
     - "Example from published writer using similar technique"
     ↓
  4. Responds: "I'll try Option B"
     ↓
  5. Revises + resubmits
     ↓
  6. Teacher reviews both versions + AI feedback
     ↓
  7. Teacher gives final feedback (builds on AI) + approval to return to student

TEACHER PATH:
  1. Sets up assignment
     ↓
  2. Customizes Langston AI rubric for this assignment
     ↓
  3. Previews agent behavior on sample essays
     ↓
  4. Deploys to students
     ↓
  5. Reviews: AI feedback + student revisions + final student draft
     ↓
  6. Gives feedback that builds on AI (not redundant)
     ↓
  7. Tracks: Did student adopt suggestions? Quality of revision?
```

### Product Feature Matrix

| Pain Point | Spiraly | Norma | Langston | Soto | Delineate | Abacus | Reclass | Exemplar | Aidstep |
|-----------|---------|-------|----------|------|-----------|--------|---------|----------|---------|
| Adaptive without removing choice | ✓ | | | | | | | | |
| Preserve student voice | | | ✓ | | | | | | |
| Calibrate rubrics | | ✓ | ✓ | | | | | | |
| ELL-first design | | | | ✓ | | | | | |
| Standards alignment | | | | | ✓ | ✓ | | | |
| Evidence-grounded feedback | | | ✓ | | | | | ✓ | |
| Role-gated access | | | | | | | ✓ | | |
| Reduce data entry friction | | | | | | | | | ✓ |

---

## Design Philosophy

### Why These 9 Agents?

Each solves a **specific K-12 instructional problem** that teachers face:

1. **Spiraly:** "How do I personalize learning without removing student choice?"
2. **Norma:** "How do I know if my rubrics are consistent? Have I drifted?"
3. **Langston AI:** "How do I give writing feedback that helps students find their voice?"
4. **Soto:** "How do I support ELL students who aren't ready to speak yet?"
5. **Delineate:** "How do I map my curriculum to standards?"
6. **Abacus:** "How do I ensure coherent math learning across grades?"
7. **Reclass:** "How do I make EL reclassification decisions defensible?"
8. **Exemplarable:** "How do I give essay feedback grounded in actual examples?"
9. **Aidstep:** "How do I help students navigate FAFSA without manual data entry?"

### What These Agents Are NOT

- **Replacement teachers:** All 9 agents require human review
- **Efficiency-first:** They prioritize student agency + voice over speed
- **Surveillance tools:** They surface aggregate patterns, not track individual students 24/7
- **Black boxes:** Every recommendation is explainable, traceable to pedagogy + data

### Design Constraints

All 9 agents follow these non-negotiable constraints:

1. **Student agency preserved:** Where possible, 2+ modalities (System-suggested, Student-choice, Hybrid)
2. **No hallucinated examples:** All examples grounded in real corpus or published sources
3. **Teacher approval required:** Agents advise, humans decide
4. **Audit trails:** Every recommendation traceable (why chosen? on what evidence?)
5. **Cultural representation:** Examples include Black, Latino, Asian-American, Indigenous voices

---

## Related Pages

- **[System Design & Architecture](./SYSTEMS.md)** - Infrastructure that enables these agent products
- **[Evaluation & Testing](./EVALS.md)** - How these agents are tested for production readiness
- **[Governance & Compliance](./GOVERNANCE.md)** - How agents comply with FERPA/COPPA

---

## Call-to-Action

**Hiring for AI product design, K-12 edtech, or user-centered AI?**

The 9 agents showcase product thinking in a regulated domain. Let's discuss how to build AI that empowers users instead of replacing them.

**Contact:** [jsunlee1013@gmail.com](mailto:jsunlee1013@gmail.com) | [LinkedIn](https://linkedin.com/in/jason-lee-60537831)

**Portfolio:** [jason-j-lee.com](https://jason-j-lee.com) - See all 9 agents in detail with interactive scenarios

---

## Code Sample: Voice-Protective Feedback Engine

```python
# Langston AI - Voice-Protective Writing Feedback
# packages/langston-ai/feedback_engine.py

from dataclasses import dataclass
from typing import List, Optional
from enum import Enum

class FeedbackType(Enum):
    CRAFT_CHOICE = "craft_choice"           # "You used repetition for emphasis"
    RHETORICAL_STRATEGY = "rhetorical_strategy"  # "Audience shifted here"
    CULTURAL_REFERENCE = "cultural_reference"    # "Code-switching observed"
    # NOT: Grammar, mechanics (teacher's job)

@dataclass
class CraftExample:
    """Real example from published or student work (never synthetic)"""
    text: str                          # The actual writing sample
    source: str                        # Author attribution
    annotation: str                    # Why this exemplifies the craft choice
    url: Optional[str] = None          # Link to full work

@dataclass
class FeedbackOption:
    """Non-prescriptive suggestion (student picks which to try)"""
    description: str                   # "You could strengthen audience clarity by..."
    exemplar: CraftExample             # Real example of this technique
    impact: str                        # Why it might help (pedagogically grounded)

class VoiceProtectiveFeedback:
    """Generate feedback that preserves student voice while building craft."""

    def analyze_voice_markers(self, text: str) -> dict:
        """Detect voice elements: repetition, dialect, code-switching, emphasis."""
        return {
            "repetition": self._find_intentional_repetition(text),
            "code_switching": self._detect_code_switching(text),
            "dialect_features": self._identify_dialect_features(text),
            "emphasis_choices": self._find_emphasis_patterns(text),
        }

    def generate_feedback(self, text: str, rubric: Rubric) -> dict:
        """
        Generate feedback that:
        1. Separates craft from correctness
        2. Offers 3-5 non-prescriptive options
        3. Grounds examples in real published/student work
        4. Explains pedagogical reasoning
        """

        voice_markers = self.analyze_voice_markers(text)
        craft_analysis = self._analyze_craft_choices(text, rubric)

        feedback = {
            "craft_observations": [],  # What the student DID (not what they missed)
            "revision_options": [],    # 3-5 non-prescriptive suggestions
            "exemplars": [],           # Real examples of similar techniques
            "reasoning": None,         # Why this feedback matters pedagogically
        }

        # Example: Student shifted audience mid-essay
        if craft_analysis.get("audience_shift"):
            feedback["craft_observations"].append({
                "observation": "You shifted audience mid-essay (line 12 → line 23)",
                "evidence": craft_analysis["audience_shift"]["evidence"],
                "question": "Intentional or opportunity to strengthen?",
            })

            # Offer options (not prescriptive)
            feedback["revision_options"].extend([
                FeedbackOption(
                    description="Maintain same audience throughout (strengthen coherence)",
                    exemplar=CraftExample(
                        text="[Real example from Maya Angelou or student work]",
                        source="Author Name",
                        annotation="Consistent audience → powerful voice"
                    ),
                    impact="Coherent voice feels stronger, reader stays engaged"
                ),
                FeedbackOption(
                    description="Intentionally shift audience (try signposting)",
                    exemplar=CraftExample(
                        text="[Real example showing intentional shifts]",
                        source="Ta-Nehisi Coates",
                        annotation="Shifts between 'you' and 'I' for rhetorical effect"
                    ),
                    impact="Deliberate shifts can deepen meaning if signposted"
                ),
                FeedbackOption(
                    description="Keep current draft (audience ambiguity as choice)",
                    exemplar=CraftExample(
                        text="[Example showing productive ambiguity]",
                        source="Ocean Vuong",
                        annotation="Ambiguous audience creates intimacy"
                    ),
                    impact="Some writers use ambiguity as a strength"
                ),
            ])

        feedback["reasoning"] = (
            "This feedback focuses on craft choices YOU made (not missing), "
            "offers options instead of imperatives, and shows published examples. "
            "You decide which direction strengthens your voice."
        )

        return feedback


    def _find_intentional_repetition(self, text: str) -> List[dict]:
        """Detect repetition patterns (often intentional for emphasis)."""
        # Implementation: regex + linguistic markers
        pass

    def _detect_code_switching(self, text: str) -> List[dict]:
        """Identify code-switching as linguistic strength."""
        # Implementation: language detection + context analysis
        pass

    def _identify_dialect_features(self, text: str) -> List[dict]:
        """Recognize dialect features (NOT as 'errors' but as voice markers)."""
        # Implementation: sociolinguistic analysis
        pass

    def _find_emphasis_patterns(self, text: str) -> List[dict]:
        """Detect emphasis techniques: repetition, inversion, pacing."""
        # Implementation: stylistic analysis
        pass

    def _analyze_craft_choices(self, text: str, rubric: Rubric) -> dict:
        """Analyze craft decisions against rubric dimensions."""
        # Implementation: craft-focused analysis
        pass
```

**Design Pattern:**
- Feedback focuses on **craft choices** (what student did), not deficits
- Every suggestion is **grounded in real examples** (no synthetic feedback)
- **3-5 options** (not 1 prescription)
- **Reasoning explained** (why this matters pedagogically)
- **Student agency:** "You decide which direction strengthens your voice"

---

**Built with:** TypeScript, Python, Claude (analysis), PostgreSQL, React

**Repository:** [00-ACTIVE-MCP-SYSTEM](https://github.com/jason-jaemin-lee/00-ACTIVE-MCP-SYSTEM) (private - available on request)
