# Learning Intelligence Dashboard

**Convert AI conversation transcripts into structured competency evidence, Bloom's Taxonomy mappings, ROI documentation, and portfolio-ready artifacts.**

[![Schema Version](https://img.shields.io/badge/Schema-v1.0-1A6B3C?style=flat-square)](schema/schema-v1.0.json)
[![Prompt Version](https://img.shields.io/badge/Prompt-v1.1-7B3FA0?style=flat-square)](prompts/session-analyzer-prompt-v1.1.md)
[![License](https://img.shields.io/badge/License-MIT-0A7EA4?style=flat-square)](LICENSE)
[![No Install](https://img.shields.io/badge/No%20Install%20Required-Open%20in%20Browser-00B4BC?style=flat-square)](#quick-start)

---

## The Problem

Billions of hours of high-quality professional learning happen inside AI chat sessions every year — applied problem-solving, strategic analysis, research, skill development. Virtually none of it leaves a verifiable record. At the same time, low-quality passive learning (the compliance module, the required slide deck) is exhaustively documented by LMS systems that record *whether training happened*, not *whether learning occurred*.

The Learning Intelligence Dashboard closes this gap.

---

## What It Does

Paste the extraction prompt at the end of any AI session. Save the JSON output. Open the dashboard. A conversation becomes a structured portfolio entry with:

- **Competency Radar** — named professional domains, scored and mapped to ATD, SHRM, SFIA, and other established frameworks
- **Bloom's Taxonomy Grid** — cognitive depth profile from surface recall through original synthesis
- **ROI Metrics** — knowledge value (USD), time efficiency multiplier, retention probability, engagement score, employer value index — all calculated via explicit, documented rubrics
- **Session Timeline** — key cognitive milestones extracted from the conversation arc
- **Employer Value Panel** — professional statements of demonstrated capability, ready for portfolio use
- **Multi-Session Aggregation** — load multiple sessions to build a cumulative competency profile across time

---

## Quick Start

**1. Run a session with any AI**
Have a substantive conversation with Claude, ChatGPT, Copilot, Gemini, Perplexity, or any capable AI model.

**2. Paste the extraction prompt**
Copy the contents of [`prompts/session-analyzer-prompt-v1.1.md`](prompts/session-analyzer-prompt-v1.1.md) and paste it at the end of your session.

**3. Save the output**
The AI will produce a JSON object. Save it as a `.json` file (e.g. `session-20260223-strategy.json`).

> ⚠️ **On platforms with limited output windows (e.g. Perplexity/Comet):** Scroll to verify the JSON closes with `}` before saving. Truncated files will fail to import.

**4. Open the dashboard**
Download [`dashboard/learning-intelligence-dashboard-v2.html`](dashboard/learning-intelligence-dashboard-v2.html) and open it in any modern browser. No server. No installation.

**5. Import your session**
Click **Add Session** and select your JSON file. Repeat for additional sessions to build an aggregated profile.

---

## Repository Structure

```
learning-intelligence-dashboard/
│
├── README.md                                  ← You are here
├── LICENSE                                    ← MIT
├── CHANGELOG.md                               ← Version history (both tracks)
├── .gitignore                                 ← Excludes sessions/ user data
│
├── dashboard/                                 ← SCHEMA TRACK — consumes schema
│   ├── learning-intelligence-dashboard-v2.html   ← Single-file app (current)
│   └── DASHBOARD-NOTES.md                        ← Parser notes, compat matrix
│
├── prompts/                                   ← PROMPT TRACK — extraction methodology
│   ├── session-analyzer-prompt-v1.1.md           ← Current canonical prompt
│   ├── session-analyzer-prompt-v1.0.md           ← Original release (retained)
│   └── PROMPTS-README.md                         ← Which prompt to use, platform notes
│
├── schema/                                    ← SCHEMA TRACK — output contract
│   ├── schema-v1.0.json                           ← Current canonical schema spec
│   └── SCHEMA-CHANGELOG.md                        ← Field-level change log
│
├── scoring/                                   ← PROMPT TRACK — methodology reference
│   └── scoring-rubrics-v1.0.md                   ← Full rubrics with citations
│
├── docs/                                      ← Project documentation
│   ├── concept-paper-v1.1.md                     ← Architecture & versioning model
│   ├── white-paper-v1.0.md                       ← Full project white paper
│   └── executive-summary-v1.0.md                 ← Two-page decision-maker summary
│
├── sessions/                                  ← USER DATA — gitignored
│   ├── sample-session.json                        ← Reference example (tracked)
│   └── README-sessions.md                         ← How to generate and name files
│
└── .github/
    └── workflows/
        └── validate-schema.yml               ← CI: validate JSON on PR (Phase 1)
```

---

## The Versioning Model

This project maintains **two independent version tracks**. Understanding their separation is fundamental to using, extending, and contributing to the system correctly.

### Schema Version — Output Contract

The `schema_version` field embedded in every session JSON file.

| Version | Status | Notes |
|---|---|---|
| `1.0` | **Current canonical** | Stable. Validated against 7 real sessions across 3 platforms. |
| `1.1` | Planned | Will add optional fields. Backward-compatible. |
| `2.0` | Future | Reserved for breaking structural changes. |

**Bump the schema version when:** the JSON output structure changes in a way that would break an existing parser — removing or renaming a field, changing a data type, or restructuring a nested object.

**Do not bump the schema version when:** adding a new optional field the parser handles gracefully when absent, or when changing how scores are calculated without changing field names or types.

### Prompt Version — Extraction Methodology

The version carried in the prompt file name and referenced in session `meta.notes`.

| Version | Status | Notes |
|---|---|---|
| `v1.0` | Legacy | General scoring guidelines only. Scores were AI-estimated with minimal constraints. Sessions produced with v1.0 conform to Schema v1.0. |
| `v1.1` | **Current canonical** | Explicit scoring rubrics embedded for all fields. `meta.notes` is required with structured calculation format. |
| `v1.2+` | Planned | May refine rubric anchors, add domain-specific variants, or support new session types. Schema output unchanged. |

**Bump the prompt version when:** scoring rubrics change, behavioral anchors are added or revised, the `meta.notes` format changes, or usage instructions affecting the workflow are updated.

**A prompt version bump does not require a schema version bump** as long as the JSON output structure is identical.

### The Key Distinction

> A session file produced by Prompt v1.0 and a session file produced by Prompt v1.1 are both Schema v1.0 files. The dashboard parses them identically. The difference lives in the `meta.notes` field — Prompt v1.1 sessions include documented scoring calculations; Prompt v1.0 sessions do not.

---

## Scoring Methodology

All scored fields in the session JSON are calculated using explicit, documented rubrics — not free-form AI estimation. The full methodology is in [`scoring/scoring-rubrics-v1.0.md`](scoring/scoring-rubrics-v1.0.md). A summary:

| Field | Method |
|---|---|
| `roi.knowledge_value_usd` | Domain rate × LMS equivalent hours × Bloom's depth multiplier |
| `roi.time_efficiency_multiplier` | LMS equivalent hours ÷ actual session hours |
| `roi.engagement_score` | Five behavioral dimensions, each 0–20, summed to 100 |
| `roi.retention_probability_pct` | 10% Ebbinghaus baseline + additive research-grounded factors, capped at 92% |
| `roi.employer_value_index` | Five professional dimensions, each 0–2.0, summed to 10.0 |
| `scores.cognitive_depth_score` | Weighted Bloom's level sum (L1–L2: 1×; L3: 1.5×; L4–L5: 2×; L6: 2.5×) |
| `scores.meta_cognition_score` | Five-band behavioral scale anchored to Flavell and Zimmerman frameworks |
| `scores.strategic_thinking_pct` | Five-band scale: 0–20% tactical through 81–100% deeply strategic |

Every session JSON produced by Prompt v1.1 includes a `meta.notes` field documenting the calculation for each scored field — creating an auditable reasoning trail embedded in the output file.

---

## Supported Platforms

The extraction prompt works with any sufficiently capable language model:

| Platform | Status | Notes |
|---|---|---|
| Claude (Anthropic) | ✅ Validated | All current models |
| Microsoft Copilot (M365) | ✅ Validated | Work and personal accounts |
| Perplexity / Comet | ✅ Validated | Verify JSON closes before saving — output window may truncate |
| ChatGPT (OpenAI) | ✅ Compatible | GPT-4o and later recommended |
| Google Gemini | ✅ Compatible | Gemini 1.5 Pro and later recommended |
| Any frontier LLM | ✅ Compatible | Must support structured JSON output |

---

## Use Cases

The system is not limited to learning and development. Any context where demonstrated professional capability needs to be observed, recorded, and communicated is a candidate.

| Domain | Application |
|---|---|
| Workforce L&D | Competency evidence from AI-assisted work sessions fills the LMS capability gap |
| Talent Acquisition | Session portfolios as evidence of thinking depth — beyond resume claims |
| Executive Coaching | Longitudinal competency growth records across a coaching engagement |
| Clinical Supervision | Applied reasoning evidence supplements formal practitioner assessment |
| Research Methodology | Documents analytical reasoning across research sessions |
| Higher Education | Student portfolio evidence beyond exams — applied learning made visible |
| Onboarding Assessment | Organic capability baseline from first-week AI work sessions |
| Legal & Compliance | Verifiable knowledge demonstration — more defensible than completion timestamps |
| Personal Development | Self-directed learning records independent of institutional enrollment |

---

## Validation Status

As of February 2026, the system has been validated against 7 real professional sessions:

| Session | Platform | Prompt | Schema | Status |
|---|---|---|---|---|
| AI-Integrated Learning Design | Claude | v1.0 | v1.0 | ✅ Parsed |
| Moodle AI Agent Blocker Plugin | Claude | v1.0 | v1.0 | ✅ Parsed |
| AI Provenance xAPI Profile | M365 Copilot | v1.0 | v1.0 | ✅ Parsed |
| AI Provenance Detection Problem | M365 Copilot | v1.0 | v1.0 | ✅ Parsed |
| Incident Response Documentation | M365 Copilot | v1.0 | v1.0 | ✅ Parsed |
| Moodle SQL Data Remediation | M365 Copilot | v1.0 | v1.0 | ✅ Parsed |
| SULMW H5P Course Design | Comet/Perplexity | v1.0 | v1.0 | ❌ Truncated — [documented](docs/known-issues.md) |

---

## Roadmap

### Phase 1 — Stabilization *(Next Priority)*
- [ ] GitHub repository with documented two-track versioning structure
- [ ] Formal `schema-v1.0.json` specification (JSON Schema draft-07)
- [ ] Schema validation on import — surface human-readable errors rather than silent failure
- [ ] Token truncation documentation and mitigation guidance for Comet/Perplexity users
- [ ] `schema_version` compatibility check in dashboard parser

### Phase 2 — Scalability & Usability
- [ ] Session persistence via localStorage (with export/import as backup)
- [ ] Search and filter across sessions by tag, date, source, competency
- [ ] Competency trend lines across sessions — track growth over time
- [ ] Bulk import (drag-and-drop multiple JSON files)
- [ ] Print/PDF export of individual session view
- [ ] Mobile-responsive improvements
- [ ] Schema migration utility (auto-convert older schema versions on import)

### Phase 3 — Use Case Expansion
- [ ] Domain-specific dashboard profiles (L&D, Talent Evaluation, Clinical, Research, Executive Coaching)
- [ ] Role-specific employer value framing (recruiter vs. hiring manager vs. learner view)
- [ ] Aggregate competency gap analysis against target role profiles
- [ ] Portfolio export — formatted PDF or HTML portfolio from selected sessions
- [ ] Team and cohort view — aggregate multiple people's sessions

### Phase 4 — Backend & Integration
- [ ] Optional lightweight backend for multi-user support
- [ ] LRS integration via xAPI — export session competency data as xAPI statements
- [ ] Moodle plugin integration
- [ ] GitHub Actions for automated schema validation on PR

---

## Contributing

Contributions are welcome. Before submitting a PR, please review the versioning model above and the guidance in [`docs/concept-paper-v1.1.md`](docs/concept-paper-v1.1.md).

**Key rules:**
- Schema changes require a version bump in `schema_version` and a parser update in the dashboard
- Prompt changes require a version bump in the file name and header — they do not require schema changes
- All scoring rubric changes must be reflected in both the prompt and `scoring/scoring-rubrics-v1.0.md`
- Session files in `sessions/` (except `sample-session.json`) should not be committed

**Types of contributions especially welcome:**
- Platform-specific prompt notes and known issues (Gemini, Mistral, local models)
- Domain-specific rubric variants (clinical, legal, research)
- Dashboard parser improvements and schema compatibility handling
- Translations of the extraction prompt
- Real-world use case documentation

---

## Schema Quick Reference

```json
{
  "schema_version": "1.0",
  "session":       { "id", "date", "title", "source", "source_type", "duration_minutes", "topic_summary", "tags" },
  "scores":        { "competency_domains_count", "cognitive_depth_score", "strategic_thinking_pct", "roi_awareness_pct", "engagement_score", "meta_cognition_score" },
  "competencies":  [{ "name", "score", "color", "frameworks", "bloom_level", "bloom_label", "tags" }],
  "radar":         { "axes": [{ "label", "value", "description" }] },
  "blooms_progression": [{ "level", "label", "icon", "title", "description", "dots_active", "dot_color" }],
  "roi":           { "knowledge_value_usd", "time_efficiency_multiplier", "engagement_score", "retention_probability_pct", "application_readiness", "employer_value_index", "lms_equivalent_hours", "session_hours" },
  "timeline":      [{ "title", "description" }],
  "employer_value": [{ "icon", "title", "description" }],
  "portfolio":     { "title", "subtitle", "primary_tags", "secondary_title", "secondary_subtitle", "secondary_tags", "documentation_formats" },
  "meta":          { "generated_by", "generated_at", "confidence", "notes" }
}
```

Full specification: [`schema/schema-v1.0.json`](schema/schema-v1.0.json)

---

## License

MIT License — see [LICENSE](LICENSE) for full text.

This system produces professional development artifacts, not credentials. Scores are calibrated estimates of demonstrated behavior in a session. They should inform portfolios and talent discussions, but should not replace validated credentialing processes in high-stakes domains.

---

## Further Reading

- [`docs/concept-paper-v1.1.md`](docs/concept-paper-v1.1.md) — System architecture, versioning model, full methodology
- [`docs/white-paper-v1.0.md`](docs/white-paper-v1.0.md) — Full project white paper
- [`docs/executive-summary-v1.0.md`](docs/executive-summary-v1.0.md) — Two-page decision-maker summary
- [`scoring/scoring-rubrics-v1.0.md`](scoring/scoring-rubrics-v1.0.md) — Complete scoring rubric definitions and research citations

---

*The conversation is the credential. The infrastructure to recognize that is finally catching up.*
