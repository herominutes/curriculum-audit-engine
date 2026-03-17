# CurriculumOS — AI Curriculum Audit Platform

> **Prototype status:** This is a functional proof-of-concept built to validate the product thesis and demonstrate senior PM thinking. It uses Claude's training knowledge as a benchmark proxy — not a live standards API. See [Validation Roadmap](#validation-roadmap) for the path to a production-grade certified product.

---

## What it does

CurriculumOS lets teachers, curriculum directors, and school administrators upload any curriculum and receive a full AI-powered audit in under 60 seconds — benchmarked against what students at that grade level, in that geography, should know to be nationally competitive in the current year.

**The core problem it solves:** EdTech companies and schools routinely test curriculum quality on real students — launching content, watching retention crater, then scrambling to fix material that learners have already abandoned. CurriculumOS moves quality assessment *before* student exposure.

---

## Live demo

Open `curriculum_audit_senior.html` directly in any modern browser. No server, no build step, no dependencies.

You'll need an **Anthropic API key** (`sk-ant-...`) to run live audits. Enter it in the persistent banner at the top — it stays in your browser only and is never logged or transmitted beyond the Anthropic API call.

To explore without an API key, hit **"Load demo curriculum"** on the New Audit screen — it pre-fills a sample 7th grade ELA curriculum you can audit immediately once a key is entered.

---

## What works now vs. what is simulated

This prototype is a single HTML file. It runs entirely in the browser. Some features make live API calls — others are populated with demo data to show what the full product looks like at institutional scale. Here is the exact breakdown so there is no ambiguity.

### ✅ Fully functional with an Anthropic API key

| Feature | What happens |
|---|---|
| New Audit | Real API call. Paste or upload any curriculum, get a live scored audit with all findings, benchmarks, action plan, and lesson stubs |
| Before / after diff (inline) | Real API call. Enable the compare toggle, paste a previous version, diff renders in the results tab with resolved/remaining/new issues |
| Standalone diff screen | Real API call. Paste two curriculum versions, get independent scores, delta, and written analysis of what drove the change |
| Lesson stub expand | Real API call. "Expand to full plan" generates a complete four-week unit plan on demand |
| Load demo curriculum | No API call needed — pre-fills a sample 7th grade ELA curriculum so you can run a live audit immediately |
| Session audit history | No API call — stores every audit you run during the session in a live timeline |

### 🟡 Populated with demo data (no API call — shows the product vision)

| Feature | What is shown | What it needs to be real |
|---|---|---|
| Dashboard KPI banner | Hardcoded school overview (avg 71, 2 below threshold) | Backend + account system with persisted audit data |
| Dashboard curriculum cards | Clicking ELA/Math loads a pre-built sample result | Real audit history stored per user account |
| Standards drift alert | Static amber banner | Webhook or scheduled job against state DOE change feeds |
| Benchmarks screen | Hardcoded state median / national avg / top quartile estimates | Live integration with Common Core API and NAEP data |
| Department view | Static teacher table with simulated scores and deltas | Multi-user account system with school-level data model |
| Historical trend (simulated) | Three hardcoded timeline entries showing a score arc | Persisted audit records with date-stamped history |

### 🔘 Export buttons (show toast, not yet wired)

| Button | What it needs |
|---|---|
| Export PDF | PDF rendering service (Puppeteer, Playwright, or a PDF API) |
| Copy share link | Backend URL generation + audit result persistence |
| Push to Notion | Notion API token + target page ID (~2 hours to implement) |
| Export department report | Backend PDF generation with multi-teacher data |

**Bottom line:** Run a live audit, use the diff engine, expand lesson stubs — those all work today with an API key. The dashboard and department views show the institutional product vision with representative data.

---

## Features

### 1. Curriculum health audit
Upload a curriculum as a text paste or file attachment. Configure grade band, subject, state/region, and school type. The engine scores 0–100 against 2026 national benchmarks and returns:

- **Health score** with live benchmark comparison (your score vs. state median, national average, top quartile)
- **Red flags** — specific gaps with standards references (Common Core, NAEP, ACT/SAT frameworks)
- **Strengths** — what is genuinely working and why
- **Missing topics** — what students at this level need that the curriculum lacks, with rationale
- **Obsolete content** — what no longer belongs and why
- **Opportunities** — specific ways to differentiate or add rigor without restructuring

### 2. Before / after diff engine
The feature that converts a one-shot tool into a recurring workflow. Two modes:

- **Inline compare** — enable the toggle in the audit form, paste your previous version, and the diff renders automatically in the results tab alongside your new score
- **Standalone diff screen** — paste any two curriculum versions, run independent scoring on both, and get a quantified delta with specific analysis of what drove the change

Every issue is tagged: resolved, remaining, or newly introduced.

### 3. Lesson stub generator
For every gap identified, the audit auto-generates a week-one lesson outline — not just a diagnosis. Each stub includes five day-by-day activities ready to use or adapt. The "Expand to full plan" button generates a complete four-week unit on demand via a second API call.

Stubs are tagged by gap type (missing topic, opportunity, red flag) and track implementation status (not started / in progress / implemented).

### 4. Benchmark intelligence
Dedicated benchmarks screen showing how your curricula compare across subjects to estimated state median, national average, and top-quartile schools. Includes a "What top-quartile schools do differently" reference list drawn from publicly available research.

> **Important:** Benchmarks are estimated from training data — not pulled from a live DOE API. See the [Validation Roadmap](#validation-roadmap).

### 5. Audit history & score timeline
Every audit in a session is stored and visualised as a timeline with score, date, and subject. The simulated historical trend shows what a multi-semester progression looks like. In production, this persists to your account and feeds department-level reporting.

### 6. Department / administrator dashboard
Principal and department-head view showing every teacher, every curriculum, with score, delta, gap count, and status in a single table. Identifies at-risk curricula and flags unaudited ones. This is the view that justifies a school-tier license to an administrator.

### 7. Product metrics screen
Every KPI defined with baseline, 90-day target, measurement method, and owner — before anything is in market. Includes an activation funnel hypothesis. This screen exists because "engagement increased" is not a metric.

### 8. Plans & pricing
Three-tier pricing hypothesis (Teacher $49/mo · School $299/mo · District custom) with a PM annotation flagging these as buyer-interview hypotheses, not final prices. Each tier lists features honestly, including what is and isn't included. The validation roadmap is embedded here so every potential buyer sees the path to a certified product.

---

## Tech stack

| Layer | Choice | Reason |
|---|---|---|
| Runtime | Vanilla HTML/CSS/JS | Zero dependencies, opens in any browser, no build pipeline |
| AI | Anthropic Claude (`claude-sonnet-4-20250514`) | Best-in-class instruction following for structured JSON output |
| Fonts | DM Sans + Instrument Serif + DM Mono (Google Fonts) | Editorial feel appropriate for an education product |
| Data persistence | Session memory (JS object) | Prototype scope — no backend required |
| File handling | FileReader API | Client-side only, no upload server needed |

Single file. 92KB. No npm. No webpack. No framework.

---

## Getting started

```bash
# Clone the repo
git clone https://github.com/your-org/curriculum-os.git
cd curriculum-os

# Open directly — no server required
open curriculum_audit_senior.html
# or on Linux:
xdg-open curriculum_audit_senior.html
# or just drag the file into Chrome/Safari/Firefox
```

1. Enter your Anthropic API key in the blue banner at the top
2. Navigate to **New Audit** in the sidebar
3. Select grade band, subject, and state
4. Paste your curriculum or click "Load demo curriculum"
5. Hit **Run audit →**

First audit result appears in approximately 15–30 seconds depending on curriculum length.

---

## Project structure

```
curriculum-os/
├── curriculum_audit_senior.html   # Entire application — single file
└── README.md                      # This file
```

All CSS, JavaScript, and HTML live in a single file by design. This is a prototype intended for rapid iteration and stakeholder demos — not a production codebase. When this graduates to production, the natural split is:

```
src/
├── components/          # Audit form, results, diff engine, stubs
├── api/                 # Anthropic client wrapper, prompt templates
├── screens/             # Dashboard, history, benchmarks, department
├── styles/              # Design system tokens, component styles
└── data/                # Standards reference data (future: live API)
```

---

## API usage

The prototype calls the Anthropic Messages API directly from the browser using `anthropic-dangerous-direct-browser-access: true`. This is intentional for a prototype — **do not use this pattern in production.** In production, API calls must be proxied through your own backend to protect the API key.

**Endpoints used:**

```
POST https://api.anthropic.com/v1/messages
Model: claude-sonnet-4-20250514
Max tokens: 1600–2000 per audit call
```

**Approximate token cost per full audit:** ~2,000–3,500 tokens input + ~1,500 tokens output ≈ $0.01–0.02 per audit at current Sonnet pricing.

---

## Prompt architecture

Each feature uses a distinct prompt persona and output schema:

| Feature | Persona | Output |
|---|---|---|
| Full audit | Senior curriculum auditor | JSON: score, red flags, strengths, missing, obsolete, opportunities, stubs, action plan |
| Diff engine | Curriculum comparison specialist | JSON: score_a, score_b, delta, resolved, remaining, new_issues, analysis |
| Lesson stub expand | Curriculum designer | Plain text: 4-week unit plan |

All audit prompts require JSON-only output with no markdown fencing, then strip any residual backticks before `JSON.parse()`. Fallback sample data renders if the API call fails so the UI never breaks during a demo.

---

## Known prototype limitations

| Limitation | Production fix |
|---|---|
| Benchmarks are AI-estimated, not authoritative | Connect Common Core API, state DOE feeds, NAEP data |
| API key exposed client-side | Proxy all calls through a backend service |
| Audit history lives in session memory only | Add account system + database persistence |
| No FERPA / COPPA compliance | Full legal review + DPA templates before any district contract |
| File uploads read as filenames only (non-text) | Server-side file parsing (PDF extraction, DOCX reader) |
| No real-time standards drift detection | Webhook or scheduled job against DOE change feeds |
| Benchmark data not geo-specific | State-level API integration per the validation roadmap |

---

## Validation roadmap

This prototype uses Claude's training knowledge as a benchmark proxy. Here is the ordered path to a certified, commercially defensible product:

**Step 1 — Live standards API integration** *(est. 8 weeks engineering)*
Connect Common Core State Standards API, state DOE published frameworks, and annual NAEP reporting. Replace the AI proxy with authoritative, versioned data that updates on each standards cycle.

**Step 2 — Parallel validation pilot** *(est. 1 semester)*
Partner with one school willing to run synthetic audits and real student outcomes simultaneously. Measure correlation between audit score and actual student performance gaps. Target: Pearson r > 0.6 before any commercial claim of predictive accuracy.

**Step 3 — Learning scientist advisory**
Hire or partner with a credentialed instructional designer or curriculum researcher to validate the audit rubric. Required to clear the internal skeptic (curriculum directors, instructional coaches) at the district procurement level. Without this, the product stalls at the teacher tier.

**Step 4 — FERPA + COPPA compliance review**
Full legal review before any district contract. Student data privacy laws are non-negotiable. Prepare Data Processing Agreement (DPA) templates before the first district sales call — procurement will request them immediately.

**Step 5 — Calibration flywheel**
Feed real student outcome data back into the audit scoring model. Each school that uses the platform improves accuracy for all users. This is the network-effect moat that a standalone AI query cannot replicate — and the primary defensibility argument against any LMS vendor shipping a native version.

---

## Product metrics

The KPIs this product must hit to demonstrate it has found product-market fit, not just user curiosity:

| Metric | 90-day target | Measurement |
|---|---|---|
| Week-2 return rate | 45% | Unique users returning after first audit |
| Audits per active user / month | 3.5 | Audit count ÷ MAU |
| Score delta after revision | +8 avg | Diff engine before vs. after |
| Lesson stub adoption | 30% | "Implemented" toggle clicks |
| Teacher → School tier conversion | 12% | Plan upgrade events |
| Time-to-first-value | < 4 minutes | Signup → first completed audit |
| NPS (teachers) | > 45 | In-app survey at day 14 + day 45 |
| Synthetic-to-real correlation | r > 0.6 | Parallel pilot · Pearson correlation |

---

## Pricing hypothesis

Three-tier model. **These are interview hypotheses — validate with 10 buyers before committing.**

| Tier | Price | Target buyer | Key unlock |
|---|---|---|---|
| Teacher | $49 / month | Individual classroom teacher | 10 audits/mo, diff engine, lesson stubs, history |
| School | $299 / month (up to 20 teachers) | Principal / curriculum director | Department dashboard, benchmark data, admin view |
| District | Custom | District superintendent / curriculum office | Live standards API, FERPA compliance, LMS integration, SLA |

The primary conversion hypothesis: a teacher who runs 3+ audits per semester and sees a measurable score improvement will upgrade to School tier to share the department view with their principal. Test this before building the upgrade flow.

---

## Competitive context

| Approach | Limitation |
|---|---|
| Annual curriculum review (manual) | Takes months, results are stale before implementation |
| Instructional coaching | High cost, low scale, inconsistent rubric |
| LMS analytics (Canvas, Moodle) | Measures engagement, not curriculum quality |
| General AI (ChatGPT, Gemini) | No structured scoring, no benchmark comparison, no history |
| This product | Structured scoring + diff + benchmarks + stubs + retention loop |

The moat is not the AI — any competitor can prompt Claude or GPT for curriculum feedback. The moat is the validated rubric, the calibration dataset from real student outcomes, and the institutional data layer built over time.

---

## From prototype to operating system — the full product vision

This prototype proves the core thesis works: AI can audit a curriculum, score it against national benchmarks, identify specific gaps, generate actionable stubs, and show improvement over time. That is the seed.

What follows is the roadmap to turn CurriculumOS into a full operating system for curriculum quality — the platform that sits at the centre of how schools plan, iterate, and improve instruction at every level.

---

### Phase 1 — Harden the core *(0–6 months)*

The single-file prototype becomes a real application with an account system and persistent data.

**Backend infrastructure**
- User authentication (teacher, admin, district roles)
- Database layer storing audit results, history, lesson stubs, and implementation tracking per user
- API proxy server — all Anthropic calls move server-side, API key never exposed client-side
- PDF export service using Puppeteer or Playwright
- Shareable audit links with unique URLs

**Standards data layer**
- Integrate Common Core State Standards API for authoritative benchmark data
- Pull annual NAEP achievement level descriptors by grade and subject
- Map ACT/SAT skill domain frameworks for grades 9–12
- Build a versioned standards database that updates when frameworks are revised
- Replace the AI benchmark proxy entirely — this is the trust foundation the product requires

**File ingestion**
- Server-side PDF text extraction
- DOCX reader for lesson plan documents
- SCORM package parser for EdTech company content
- Video transcript ingestion (for course-based curricula)

---

### Phase 2 — Institutional product *(6–12 months)*

The product grows from individual teacher utility to a school and district operating system.

**Multi-user school workspace**
- Department dashboards become live, not demo — fed by real audit data from every teacher in the school
- Admin view with drill-down from school → department → individual curriculum
- Audit scheduling — set a cadence and the system reminds teachers when a re-audit is due
- Curriculum version control — every revision stored, every diff available, full change history

**LMS integration**
- Canvas integration — audit curricula directly from course shell
- Google Classroom connector — pull course content for audit without copy-paste
- Moodle and Blackboard support for higher ed
- SCORM file upload and analysis for EdTech companies and test prep firms

**Collaborative review workflows**
- Department head review queue — teacher submits audit, department head approves or flags
- Comment and annotation layer on audit findings
- Curriculum improvement tasks assigned to teachers with due dates and status tracking
- Principal sign-off workflow before curriculum is marked approved

**Notification and drift detection**
- Automated alerts when state standards update — shows which of your curricula are affected and by how much
- Scheduled re-audit reminders at configurable intervals
- Score regression alerts — if a curriculum's score drops after a revision, flag it immediately

---

### Phase 3 — Intelligence layer *(12–24 months)*

The platform accumulates data across schools and begins to generate insights that no individual school could produce alone.

**Calibration flywheel**
- Partner with schools to collect real student outcome data (assessment results, SBAC scores, NAEP participation)
- Build a correlation model: does audit score predict student performance?
- Target Pearson r > 0.6 between curriculum health score and standardised assessment outcomes
- Each school that contributes data improves accuracy for every school on the platform
- This is the moat — it cannot be replicated by a competitor starting from zero

**Peer benchmarking (real data)**
- Replace estimated benchmarks with actual anonymised data from schools on the platform
- "Schools similar to yours in California with a Title I designation average 68 on this subject" — not an estimate
- Subject-specific benchmark breakdowns: your ELA score vs. comparable schools by grade band, geography, school type
- Opt-in ranking for schools that want competitive visibility

**Predictive gap detection**
- Trend analysis: if a curriculum's score has declined for two consecutive semesters, surface a predictive alert before it becomes a problem
- Cross-subject gap mapping: students who are weak in data literacy in math are also likely weak in evidence-based writing in ELA — the system surfaces cross-department patterns
- Grade band continuity analysis: track whether what is taught in grade 6 adequately prepares students for grade 7 standards

**AI curriculum co-pilot**
- Interactive audit refinement: ask the AI why a specific finding was flagged and get a detailed explanation with standards citations
- "What if" scenario planning: "If I add these three topics, what would my estimated score be?"
- Curriculum generation assistance: given a gap list and grade level, draft a full unit from scratch — not just a week-one stub
- Alignment checker: paste a lesson plan and a standards list, get an automated alignment map

---

### Phase 4 — Market expansion *(24+ months)*

The platform extends beyond K-12 to adjacent markets with the same core problem.

**Higher education**
- Course audit for university departments
- Accreditation alignment checking (regional accreditors, ABET, AACSB, etc.)
- Curriculum mapping across a degree program — does the sequence of courses build skills coherently?

**EdTech companies and test prep firms**
- The original thesis: stress-test course content before it reaches real learners
- Upload a full course (video transcripts, assessments, reading materials)
- Synthetic learner simulation against defined learner profiles
- Audit against certification exam frameworks (CompTIA, AWS, SHRM, etc.)
- Before/after scoring across product iterations — replaces expensive A/B testing on real users

**Corporate learning and development**
- Audit internal training content against current skills frameworks (LinkedIn Skills Graph, O*NET)
- Flag obsolete compliance training content
- Benchmark L&D curricula against industry peer data
- Skills gap analysis: map training content to the skills the business has defined as strategic

**International expansion**
- Support for non-US standards frameworks (UK National Curriculum, IB Programme, Australian Curriculum)
- Multi-language curriculum ingestion
- Country-specific benchmark data partnerships

---

### Technology architecture at scale

When the prototype graduates to a production platform, the architecture looks like this:

```
┌─────────────────────────────────────────────────────┐
│                   CurriculumOS Platform              │
├─────────────┬───────────────┬────────────────────────┤
│  Web app    │  Mobile app   │  LMS plugins           │
│  (React)    │  (React       │  (Canvas, Classroom,   │
│             │   Native)     │   Moodle, Blackboard)  │
├─────────────┴───────────────┴────────────────────────┤
│                    API Gateway                        │
├──────────────┬──────────────┬────────────────────────┤
│  Audit       │  Standards   │  User &                │
│  Service     │  Service     │  Account Service       │
│  (Claude     │  (Common     │  (Auth, roles,         │
│   API proxy) │   Core API,  │   billing)             │
│              │   NAEP, DOE) │                        │
├──────────────┼──────────────┼────────────────────────┤
│  File        │  Benchmark   │  Notification          │
│  Ingestion   │  Intelligence│  Service               │
│  Service     │  Service     │  (drift alerts,        │
│  (PDF, DOCX, │  (calibration│   reminders)           │
│   SCORM)     │   model)     │                        │
├──────────────┴──────────────┴────────────────────────┤
│              Data layer                               │
│  PostgreSQL (audit records, user data, history)       │
│  Vector DB (curriculum embeddings for similarity)     │
│  S3 / blob storage (uploaded files)                   │
│  Redis (session cache, rate limiting)                 │
└─────────────────────────────────────────────────────┘
```

**Key engineering decisions at scale:**
- All AI calls proxied server-side — no client-side API key exposure
- Curriculum content stored as embeddings to enable similarity search ("find me curricula like this one")
- Standards data versioned in the database — audits are reproducible against the exact standards version active at audit time
- Multi-tenant data model with strict school-level isolation — no cross-school data leakage
- SOC 2 Type II certification path begins at Phase 2, required for district contracts

---

### What needs to happen before this is a real company

In order of priority:

1. **Run the parallel validation pilot.** One school. One semester. Real student outcomes alongside synthetic audit scores. Without a correlation number, every claim about predictive accuracy is a hypothesis. This is the most important thing.

2. **Hire a learning scientist.** Not an advisor — a full-time hire or a very engaged fractional role. They validate the rubric, give the product academic credibility, and neutralise the internal skeptic at district procurement. This is the hire that unlocks the School and District tiers.

3. **Get 10 paying teachers before building anything else.** $49/month. Real credit card. Real curriculum. The question is not whether teachers find it interesting — it is whether they pay for it and come back. Validate the retention loop before scaling the infrastructure.

4. **Legal review.** FERPA and COPPA before any student data enters the system. Data Processing Agreements before the first district call. This is not optional and not last-minute work.

5. **Standards API integration.** Replace the AI benchmark proxy with authoritative data. This is the technical work that makes the product defensible to a curriculum director asking "where do these benchmarks come from?"

---

## Contributing

This is an active prototype. The highest-value contributions right now are:

1. **Standards data integration** — if you have experience with Common Core API or state DOE data feeds, open an issue
2. **Rubric validation** — if you are a curriculum designer or instructional coach, critique the scoring logic in the audit prompt (`runAudit()` function)
3. **Buyer research** — if you work in EdTech, K-12 administration, or test prep, feedback on the pricing hypothesis and feature prioritisation is more valuable than code right now

---

## Disclaimer

CurriculumOS is a prototype built to validate a product thesis. It is not a certified curriculum audit tool. Findings should be treated as an AI-informed first draft, not authoritative guidance. Do not make curriculum decisions affecting students based solely on this tool's output without independent review by a qualified educator.

---

## License & intellectual property
### Copyright © 2026. All rights reserved.

This repository and everything in it — the source code, product architecture, audit engine, scoring logic, prompt design, pricing model, and product strategy — is the original intellectual property of the repository owner. It is published here for two explicit purposes: to attract conversation with investors, EdTech operators, curriculum directors, and potential co-founders who see the commercial potential in this space; and to demonstrate a complete senior product thinking methodology, from problem definition through retention design, metric discipline, validation roadmap, and competitive moat analysis.
This is not open-source software. Publication on GitHub does not constitute a transfer of IP rights or an invitation to commercialise. You may view, study, and reference this work for educational and non-commercial purposes with attribution. You may not use the code, architecture, scoring rubric, or product strategy in any commercial product or service without a separate written agreement with the author.
If you are an investor, operator, or potential co-founder interested in building this — reach out directly.

---

*Built as a senior PM prototype demonstrating: customer problem clarity, retention-loop design, metric discipline before launch, honest validation roadmap, and pricing hypothesis thinking. See [Product Metrics](#product-metrics) and [Validation Roadmap](#validation-roadmap) for the full product strategy.*
