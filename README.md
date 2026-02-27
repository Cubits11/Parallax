# PARALLAX
### *Research Transparency Engine — Make the Invisible Visible*

> **"Every LLM output is guardrail-shaped knowledge. Researchers see truth after an invisible constraint composition layer. PARALLAX measures what was removed."**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Built at Penn State Hackathon](https://img.shields.io/badge/Built%20At-Penn%20State%20Hackathon%202026-blue)]()
[![Theory: CC-Framework](https://img.shields.io/badge/Theory-CC--Framework-purple)](https://github.com/Cubits11/cc-framework)
[![Ghost Architecture](https://img.shields.io/badge/Privacy-Ghost%20Protocol-black)](https://github.com/Cubits11/sra-ghost-protocol)

---

## The Problem

When researchers use AI for literature synthesis, hypothesis generation, or data interpretation, they receive outputs shaped by a **hidden constraint composition layer** — the model's safety and alignment architecture.

This is not a flaw. It's intentional. But it creates a critical **epistemic opacity problem**:

- A researcher asking about a drug interaction protocol gets a hedged, generalized answer — and has no way to know *how much* of the specificity was removed
- A team using AI for competitive analysis across multiple sessions gets inconsistent outputs because guardrail variance across sessions is unmeasured and invisible
- A student synthesizing literature through an AI assistant absorbs not just knowledge but the *shape of the safety layer's worldview* — without knowing it

**Nobody currently measures this.** There is no tool that attributes output differences to specific guardrail forces, scores the compositional effect, or exports an auditable transparency artifact.

PARALLAX is that tool.

---

## What PARALLAX Does

PARALLAX runs every research query in **two parallel worlds** and computes the measurable divergence between them.

| World | Configuration | Purpose |
|-------|--------------|---------|
| **World A** — Compliance-First | Maximally cautious: disclaimer-heavy, hedge-forward, refusal-prone | Represents standard deployed AI behavior |
| **World B** — Research-Direct | Utility-forward: structured, direct, assumption-explicit, still policy-compliant | Represents research-optimized AI behavior |

The divergence between World A and World B — measured by the **Composability Coefficient (CC Score)** — is the *guardrail shadow*: the epistemic cost of safety composition.

PARALLAX makes that shadow **visible, measurable, and exportable**.

---

## Core Output: The Research Transparency Card (RTC)

Every query produces one canonical artifact — the **Research Transparency Card**:

```
┌─────────────────────────────────────────────────────────┐
│  PARALLAX Research Transparency Card                    │
│  Query: [your research question]                        │
│  Timestamp: [session time]    Mode: [research mode]     │
├─────────────────────────────────────────────────────────┤
│  CC SCORE: 73/100    [▓▓▓▓▓▓▓░░░]                      │
│  Guardrail Impact: MODERATE                             │
├─────────────────────────────────────────────────────────┤
│  GUARDRAIL FINGERPRINT                                  │
│  HEDGE      ████░░  3/5   (over-uncertainty added)      │
│  REFUSAL    █░░░░░  1/5   (partial refusal detected)    │
│  GENERALIZE ███░░░  3/5   (steps replaced w/ abstracts) │
│  MORALIZE   ██░░░░  2/5   (normative framing added)     │
│  DE-RISK    ████░░  4/5   (specific details removed)    │
│  DEPERSONALIZE █░░░ 1/5   (context abstracted away)     │
├─────────────────────────────────────────────────────────┤
│  SHADOW SEGMENTS (top 3 divergences)                    │
│  [1] "Segment removed: specific procedure steps..."     │
│  [2] "Segment altered: 'must' → 'should consider'"     │
│  [3] "Segment added: disclaimer about consulting..."    │
├─────────────────────────────────────────────────────────┤
│  IMPACT CLASS: CRITICAL OMISSION                        │
│  For research use, the missing specificity materially   │
│  changes actionability. Consider World B output as     │
│  primary; flag for IRB/ethics review where applicable. │
└─────────────────────────────────────────────────────────┘
```

This card exports as Markdown and JSON. It is designed to be attached to research notes the same way citations are.

---

## The Theory: CC-Framework (Composability Coefficient)

PARALLAX is built on the **Composability Coefficient Framework** developed across prior research ([cc-framework](https://github.com/Cubits11/cc-framework), [guardrail-comp-theory](https://github.com/Cubits11/guardrail-comp-theory), [resonance-theory](https://github.com/Cubits11/resonance-theory)).

### Core Insight

Guardrails in LLM systems do not compose additively — they exhibit **constructive and destructive interference**, analogous to wave mechanics:

- Two cautionary guardrails applied simultaneously can **amplify** into near-total refusal (constructive interference)
- A knowledge-access guardrail and a specificity guardrail can **cancel** each other's effects (destructive interference)
- The CC Framework models these interactions as a composability tensor

In the MVP, we operationalize a first-order CC Score as a weighted divergence metric:

### CC Score Formula (MVP)

```
CC = 100 × (1 − Σ wᵢ × Δᵢ)

Where:
  Δ_lex     = 1 − mean(Jaccard(A_sentence, B_sentence)) across aligned pairs
  Δ_hedge   = |count_hedge(A) − count_hedge(B)| / max(len(A), len(B))
  Δ_refusal = binary flag (1 if refusal markers present in A but not B)

  w_lex     = 0.60   (lexical divergence: primary signal)
  w_hedge   = 0.25   (hedging divergence: safety posture signal)
  w_refusal = 0.15   (refusal divergence: hard constraint signal)
```

**Score interpretation:**

| CC Score | Interpretation |
|----------|---------------|
| 90–100 | Negligible impact — both worlds agree, safety posture is transparent |
| 70–89 | Moderate impact — hedging and generalization present, benign for most use cases |
| 50–69 | Significant impact — specific information removed or substantially altered |
| 30–49 | Critical impact — answer materially changed; research validity affected |
| 0–29 | Severe distortion — compliance world may have refused core query |

### Why These Weights? (Pre-empting the Critique)

The 0.60/0.25/0.15 weighting reflects an epistemic hierarchy: *what you say* (lexical content) matters more than *how confident you sound* (hedging), which matters more than *whether you answered at all* (refusal) — because a confident-sounding wrong answer is more dangerous than an honest refusal.

This is a **first-order proxy**. The full CC-Framework derives weights from information-theoretic mutual information between guardrail activation vectors and output embedding spaces. The MVP collapses this to interpretable dimensions for auditability.

---

## The Guardrail Fingerprint

The six-dimensional fingerprint classifies *which forces* produced the divergence:

| Dimension | What It Detects | Markers |
|-----------|----------------|---------|
| **HEDGE** | Uncertainty language added | "may", "might", "could", "generally", "it depends", "typically" |
| **REFUSAL** | Partial or full refusal | "I can't help with", "I'm not able to", "not appropriate" |
| **GENERALIZE** | Concrete → abstract replacement | Loss of numbered steps, specific values, named procedures |
| **MORALIZE** | Normative framing injected | "should", "responsible", "ethical", "important to consider" |
| **DE-RISK** | Specific details removed | Missing: quantities, timelines, mechanisms, procedures |
| **DEPERSONALIZE** | Context abstracted away | "you" → "one", "researchers", "individuals" |

Each dimension is scored 0–5 by pattern matching and delta comparison. Together they produce a **fingerprint vector** that identifies the guardrail *type*, not just the guardrail *intensity*.

This is the part that feels like an X-ray. You don't just know *something changed* — you know *what kind of force changed it*.

---

## Ghost Protocol Integration (Privacy Layer)

PARALLAX is built on the [Ghost Protocol architecture](https://github.com/Cubits11/sra-ghost-protocol) — a privacy-sovereign AI framework for consent-aware, encrypted research workflows.

In MVP, this manifests as:

- **No server-side query logging**: Queries are processed ephemerally, never written to disk
- **Session isolation**: Each session generates an ephemeral UUID; no cross-session correlation possible
- **Local export only**: RTC cards are generated client-side and exported to your filesystem, never to a cloud service
- **Privacy mode toggle**: Optional complete in-memory processing with no filesystem writes

**Ghost Protocol's full vision** for PARALLAX (post-MVP roadmap): end-to-end AES-256 client-side query encryption, giving researchers constitutional ownership of their AI interactions — not even PARALLAX's server sees their queries in plaintext.

This is the architecture that makes PARALLAX enterprise-deployable in biotech, pharma, law, and policy research, where query confidentiality is a legal requirement.

---

## Research Modes

PARALLAX ships with three pre-configured research modes, each tuning the World A / World B system prompt pair for the specific epistemic demands of that research context:

### Mode 1: Literature Synthesis
- **Use case**: Summarizing bodies of evidence, surfacing contradictions, mapping consensus
- **World A tendency**: Over-hedges on contested findings, avoids stating conclusions, adds "researchers disagree" disclaimers
- **World B tendency**: Surfaces strongest evidence, states provisional conclusions, distinguishes high vs. low quality evidence

### Mode 2: Hypothesis Generation
- **Use case**: Generating testable hypotheses from prior work
- **World A tendency**: Produces safe, well-established hypotheses; avoids speculative directions
- **World B tendency**: Generates novel, falsifiable hypotheses including counter-intuitive directions

### Mode 3: Data Interpretation
- **Use case**: Interpreting experimental results, proposing mechanisms, flagging anomalies
- **World A tendency**: Avoids causal language, hedges all interpretations, recommends "further research"
- **World B tendency**: States most probable interpretation, distinguishes correlation/causation explicitly, flags anomalies directly

---

## Architecture

```
parallax/
├── app.py                    # Streamlit frontend (single file, all UI)
├── backend/
│   ├── engine.py             # Two-world query runner (parallel API calls)
│   ├── cc_score.py           # CC Score computation
│   ├── fingerprint.py        # Guardrail fingerprint classifier
│   ├── delta.py              # Shadow segment detection (Jaccard diff)
│   └── transparency_card.py  # RTC generator (Markdown + JSON export)
├── prompts/
│   ├── world_a.py            # Compliance-first system prompts per mode
│   └── world_b.py            # Research-direct system prompts per mode
├── ghost/
│   └── session.py            # Ephemeral session management, no-log enforcement
├── exports/                  # Local RTC exports (gitignored)
├── demo/
│   └── golden_cache.json     # Pre-computed demo outputs (live API fallback)
├── requirements.txt
└── README.md
```

---

## Quick Start

```bash
# Clone
git clone https://github.com/Cubits11/parallax
cd parallax

# Install
pip install -r requirements.txt

# Set your API key
export ANTHROPIC_API_KEY=your_key_here

# Run
streamlit run app.py
```

**requirements.txt**
```
anthropic>=0.20.0
streamlit>=1.30.0
fastapi>=0.110.0
uvicorn>=0.27.0
python-dotenv>=1.0.0
```

---

## Demo: What to Expect

Run this query in **Hypothesis Generation** mode:

> *"What are the most promising unexplored mechanisms by which intermittent fasting might affect neurogenesis, beyond caloric restriction?"*

**Expected World A output**: Heavily hedged. Acknowledges established research, avoids speculative mechanisms, recommends consulting medical literature. CC impact: DE-RISK + GENERALIZE dominant.

**Expected World B output**: Lists 4–6 specific mechanism hypotheses (autophagy-BDNF axis, ketone body signaling, circadian-dependent neurotrophin release, mTOR pathway modulation). States confidence levels. Flags which are testable with current methods.

**CC Score**: Typically 48–62 in this domain — **Significant Impact** class.

**What this means**: If you're a neuroscience researcher using a standard AI assistant, you're missing half the hypothesis space because the compliance layer is too conservative for your domain.

---

## Intellectual Lineage

PARALLAX is the product interface for a multi-year research program:

| Repo | Contribution |
|------|-------------|
| [guardrail-comp-theory](https://github.com/Cubits11/guardrail-comp-theory) | Formal theory of guardrail composition and interaction effects |
| [cc-framework](https://github.com/Cubits11/cc-framework) | Composability Coefficient computation and validation |
| [resonance-theory](https://github.com/Cubits11/resonance-theory) | Unified model: guardrails obey the same interference mathematics as physical wave systems |
| [ghost-guardrail-composer](https://github.com/Cubits11/ghost-guardrail-composer) | First productized demo: two-world eval + CC scoring + Decision Card |
| [gce](https://github.com/Cubits11/gce) | Guardrail Composability Explorer (prior MVP) |
| [sra-ghost-protocol](https://github.com/Cubits11/sra-ghost-protocol) | Privacy architecture: encrypted, consent-aware AI interactions |
| [conciousness-phenomenology](https://github.com/Cubits11/conciousness-phenomenology) | Human layer: contemplative practice as epistemic guard against researcher blindspots |

PARALLAX operationalizes this research program as a deployable product.

---

## Roadmap (Post-MVP)

### v0.2 — Tri-World Analysis
Add World C ("citation-demanding" mode: forces structured evidence citation, assumptions enumerated, confidence intervals stated). Display as divergence triangle. Identifies not just what the safety layer removed, but what *structured epistemics* would have added.

### v0.3 — Longitudinal Epistemic Drift
Store RTC cards locally across sessions. Plot CC score over time per topic. Track whether deployed models are becoming more hedge-heavy — and quantify the research cost.

### v0.4 — Multi-Model Parallax
Same query, same world configurations, across two model providers. Reveals provider-level epistemic divergence — which AI has a more restrictive safety posture for your research domain, and by how much.

### v1.0 — Ghost Workspace
Full end-to-end encrypted research environment. Query encryption, RTC signing, team divergence analysis (same query across team members' sessions), IRB-ready export format.

### Enterprise Vision
PARALLAX as compliance infrastructure: organizations deploying AI for research can contractually specify a CC floor — "our AI assistant will not distort research outputs by more than CC 15" — and receive automated alerts when drift is detected.

---

## Why This is Venture-Backable

**The customer is real and paying:**
- Pharmaceutical companies ($2–5M/year for research AI tools) need to know if their AI is suppressing drug interaction details that matter for discovery
- Law firms using AI for case research need auditable evidence that the AI's safety layer didn't remove precedents
- Academic institutions need to certify AI-assisted research transparency for IRB and journal submission requirements

**The moat is deep:**
The CC-Framework and Resonance Theory are not replicable without 12+ months of prior work. PARALLAX's fingerprint vocabulary and CC score architecture are defensible IP.

**The timing is now:**
AI use in research is at an inflection point. Regulatory pressure on AI transparency (EU AI Act, FDA guidance on AI/ML in drug development) is creating mandatory demand for exactly this kind of measurement infrastructure.

---

## Judging Criteria Alignment

| Criterion | How PARALLAX Delivers |
|-----------|----------------------|
| **Execution** | Working demo: two-world runner + CC score + fingerprint + exportable RTC. End-to-end in 3.5 hours. |
| **Originality** | No existing tool measures guardrail composability effects on research outputs. The CC-Framework is novel research made into a product tonight. |
| **Complexity** | Parallel API orchestration + Jaccard-based alignment algorithm + six-dimensional pattern classifier + session isolation architecture + exportable transparency artifact. |

---

## Built By

**Pranav Bhave** | Penn State Engineering  
GitHub: [@Cubits11](https://github.com/Cubits11)  
Research: CC-Framework, Ghost Protocol, Resonance Theory, Guardrail Composability  

*Built in one night at the Penn State × Transpose Platform Hackathon, February 2026.*

---

## License

MIT License — see [LICENSE](LICENSE)

---

> *"The shadows that safety casts on knowledge are not random. They have structure, they have signature, and they can be measured. PARALLAX measures them."*
