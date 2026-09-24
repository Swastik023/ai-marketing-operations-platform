# Getting Started with OmniGrowth Engine

**Version 3.25.0** | A plugin for Claude Code and Claude Cowork

OmniGrowth Engine transforms Claude into a marketing command center that knows your brand, understands your industry, and produces strategy and content that sounds like you wrote it. v3.0 adds a **12-Part Engagement Methodology** that orchestrates the plugin into a sequential workflow producing ~50–60 traceable files per engagement. This guide walks you through installation, brand setup, your first marketing task, and your first full engagement.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Installation](#2-installation)
3. [First Run --- What Happens](#3-first-run--what-happens)
4. [Your First Brand Profile](#4-your-first-brand-profile)
5. [Importing Your Brand Guidelines (Optional)](#5-importing-your-brand-guidelines-optional)
6. [Your First Marketing Task](#6-your-first-marketing-task)
   - [Evaluation & Quality Assurance](#evaluation--quality-assurance)
   - [Multilingual Support](#multilingual-support)
7. [Your First Full Engagement (v3.0)](#7-your-first-full-engagement-v30)
8. [Understanding the Session Lifecycle](#8-understanding-the-session-lifecycle)
9. [Python Dependencies (Optional)](#9-python-dependencies-optional)
10. [Connector Discovery](#10-connector-discovery)
11. [Available Commands](#11-available-commands)
12. [Next Steps](#12-next-steps)

---

## 1. Prerequisites

You need exactly one thing to get started:

- **Claude Code** installed and working on your machine (macOS, Windows, or Linux), **or**
- **Claude Cowork** (part of Claude Desktop on macOS and Windows --- requires Claude Pro, Max, Team, or Enterprise)

That is it. Everything else is optional.

**Optional but nice to have:**

- **Python 3.8 or newer** --- unlocks advanced scoring features like brand voice analysis and content readability. The plugin works perfectly without Python; you just get bonus capabilities if it is installed.
- **No API keys required** --- the plugin ships with 169 reference knowledge files that power all 16 marketing modules (including the v3.0 methodology and framework reference docs). The optional MCP integrations (10 registry-backed HTTP connectors that work in Cowork, plus a 68-server opt-in catalog for Claude Code) use your own account credentials and can be configured later. Run `/omni-growth-engine:integrations` to see which connectors are available and `/omni-growth-engine:connect <name>` for step-by-step setup.

> **Bottom line:** If you can run Claude Code or Claude Cowork, you can use this plugin right now.

---

## 2. Installation

You have three options depending on which Claude interface you use.

### Option A: Install in Claude Code (from a local directory)

If you have the plugin files on your machine (downloaded or cloned):

```
claude plugin add /path/to/omni-growth-engine
```

On Windows, that might look like:

```
claude plugin add "C:\Users\yourname\Downloads\omni-growth-engine"
```

### Option B: Install in Claude Code (from the plugin registry)

If the plugin has been published to the Claude plugin registry:

```
claude plugin add omni-growth-engine
```

### Option C: Install in Claude Cowork

If you use Claude Cowork (the agentic mode in Claude Desktop):

1. Compress the `omni-growth-engine/` folder into a ZIP file
2. Open Cowork in Claude Desktop
3. Click **Plugin** in the left sidebar
4. Click **+** then **Upload**
5. Select your ZIP file

Or browse the [Claude plugin marketplace](https://claude.com/plugins) directly from Cowork: click **Plugin** → **+** → **Browse plugins** → search for "OmniGrowth Engine."

> **Cowork note:** After installing, Cowork will ask for permission to access `~/.claude-marketing/` when it first tries to read or write brand data. Grant this permission --- it is where your brand profiles and campaign history are stored.

For full details on Cowork capabilities (document creation, visual review, app integration), see the [Claude Interfaces Guide](claude-interfaces.md#claude-cowork-full-support).

### What successful installation looks like

After running either command, you should see output similar to this:

```
Installing plugin: omni-growth-engine v3.25.0
  - 16 marketing modules loaded
  - 163 skills + 18 top commands registered (/omni-growth-engine:*)
  - 24 specialist agents available
  - 10 HTTP connectors + a 68-server opt-in catalog available
  - Hooks ship empty (opt-in SessionStart/PreToolUse/SessionEnd reference config in hooks/hooks-reference.example.json)
  - 12-Part Engagement Methodology available (run /omni-growth-engine:engagement to start)

Plugin "omni-growth-engine" installed successfully.
Run /omni-growth-engine:brand-setup to create your first brand profile, then
/omni-growth-engine:engagement start <brand> <id> for a full engagement workflow.
```

If you see an error instead, verify that your Claude Code installation is up to date and that the path to the plugin directory is correct.

---

## 3. First Run --- What Happens

When you start a new session in Claude Code or Cowork after installing the plugin, you pull your brand context into the session. Here is what that looks like.

> **Note (v3.1+):** Hooks ship **empty** by default, so nothing runs automatically on session start — this keeps the plugin from interfering with non-marketing work in other projects. The brand banner below appears when you run `/omni-growth-engine:status` (or the first time any skill loads brand context). To have it injected automatically at the start of *every* session, copy the `SessionStart` entry from `hooks/hooks-reference.example.json` into `hooks/hooks.json`.

### The startup sequence (when the SessionStart hook is enabled, or you run `/omni-growth-engine:status`)

1. **The trigger** --- either the SessionStart hook fires (if you re-enabled it) or you run `/omni-growth-engine:status`.

2. **The setup script runs** --- it checks for dependencies and looks for your active brand profile (`setup.py --check-deps --summary`).

3. **You see a status banner** --- the output depends on whether you have a brand profile set up yet.

### If you have not set up a brand yet

You will see this:

```
=== DIGITAL MARKETING PRO ===
No active brand. Run /omni-growth-engine:brand-setup to create one.
===
```

This is your cue to create your first brand profile (covered in the next section).

### If you already have an active brand

You will see a 15-line brand summary that looks something like this:

```
=== DIGITAL MARKETING PRO ===
Brand: Greenfield Coffee Roasters (greenfield-coffee-roasters)
Industry: Food & Beverage (regulated: no)
Model: B2C_DTC | Revenue: transactional
Voice: Formality 4/10 | Energy 7/10 | Humor 4/10 | Authority 6/10
Traits: warm, knowledgeable, passionate
Channels: email (primary), instagram
Markets: US | Compliance: FTC, CAN-SPAM
Goals: Grow subscriber list by 40% in Q2
Competitors: Blue Bottle, Counter Culture, Stumptown
Active campaigns: 2 | Tracked insights: 14
Python: lite (nltk, textstat)
MCP: Google Analytics, Mailchimp connected
===
```

This summary is not just for you to read --- it is injected directly into the session context so that every marketing response from that point forward is grounded in your brand's voice, industry, compliance requirements, and strategic goals.

---

## 4. Your First Brand Profile

Let us walk through setting up a brand from scratch. For this example, we will create a profile for **Greenfield Coffee Roasters**, a direct-to-consumer specialty coffee brand.

### Starting brand setup

Type the following in your Claude Code or Cowork session:

```
/omni-growth-engine:brand-setup
```

The plugin will walk you through an interactive profiling conversation. By default, it uses **Quick Setup** mode, which asks just 5 essential questions.

### Quick Setup walkthrough

Here is what the conversation looks like:

```
You: /omni-growth-engine:brand-setup

Plugin: I'll help you set up your brand profile. Let's start with the essentials.

         Quick Setup (5 questions)
         -------------------------

         1. What's your brand or business name?
You:     Greenfield Coffee Roasters

         2. In one sentence, what does Greenfield Coffee Roasters do?
You:     We roast and sell single-origin specialty coffee direct to consumers online

         3. Who is your primary customer?
You:     Coffee enthusiasts aged 25-45 who care about sourcing and sustainability

         4. Pick 3 words that describe how your brand communicates:
You:     Warm, knowledgeable, passionate

         5. Where do you primarily market? (social media, email, SEO, paid ads, etc.)
You:     Email and Instagram
```

### What happens behind the scenes

From those 5 answers, the plugin intelligently builds a complete brand profile:

- **Industry** detected as Food & Beverage based on your description
- **Business model** identified as B2C DTC (direct to consumer, transactional revenue)
- **Compliance rules** loaded for FTC endorsement guidelines and CAN-SPAM (email marketing)
- **Voice dimensions** mapped from your 3 words:
  - "Warm" translates to Formality 4/10, Humor 4/10
  - "Knowledgeable" translates to Authority 6/10
  - "Passionate" translates to Energy 7/10
- **Channel specs** loaded for email (subject line limits, preview text guidelines) and Instagram (caption lengths, hashtag best practices)
- **Sensible defaults** set for everything else (you can refine later)

### The confirmation

After processing, you see your brand summary:

```
Brand profile created!

Brand: Greenfield Coffee Roasters (greenfield-coffee-roasters)
Industry: Food & Beverage (regulated: no)
Model: B2C_DTC | Revenue: transactional
Voice: Formality 4/10 | Energy 7/10 | Humor 4/10 | Authority 6/10
Traits: warm, knowledgeable, passionate
Channels: email (primary), instagram
Markets: US | Compliance: FTC, CAN-SPAM
Saved to: ~/.claude-marketing/brands/greenfield-coffee-roasters/profile.json

Quick profile created! You can refine it anytime with /omni-growth-engine:brand-setup --full
```

### Where your profile lives

Your brand data is stored locally on your machine at:

```
~/.claude-marketing/brands/greenfield-coffee-roasters/profile.json
```

This is a persistent location outside the plugin directory, so your brand profiles survive plugin updates.

### Want more detail? Use Full Setup

The Quick Setup is great for getting started fast, but if you want a more thorough profile, run:

```
/omni-growth-engine:brand-setup --full
```

Full Setup asks 17 questions across 6 categories:

| Category             | Questions | What it captures                                        |
|----------------------|-----------|---------------------------------------------------------|
| Brand Identity       | 4         | Name, elevator pitch, USP, mission and values           |
| Business Model       | 3         | Business type, revenue model, price range, sales cycle  |
| Industry & Compliance| 3         | Industry, regulated status, target markets              |
| Brand Voice          | 4         | Voice dimensions (1-10 scales), personality traits, this-not-that examples, sample content |
| Channels & Goals     | 2         | Active channels, primary goal, KPIs, budget, team size  |
| Competitors          | 1         | 3-5 competitors with strengths and weaknesses           |

You can also run Full Setup later to fill in sections you skipped. It will preserve your existing answers and only ask about what is missing.

---

## 5. Importing Your Brand Guidelines (Optional)

If your brand has a style guide, messaging framework, restriction list, or channel-specific rules, you can import them now. Guidelines go beyond the numeric voice scores in your brand profile — they capture the detailed rules that make content authentically on-brand.

```
/omni-growth-engine:import-guidelines
```

You can paste content from existing documents or describe rules conversationally:

```
You: Here's our brand voice guide: We're friendly but professional.
     Never use jargon. Always explain technical concepts simply.
     Sentences should be under 20 words.
```

The plugin extracts the rules, structures them into the right category, and saves them. They are then enforced automatically when creating content.

**What you can import:**
- **Voice & tone rules** — writing style, dos/don'ts, readability rules
- **Restrictions** — banned words, restricted claims, mandatory disclaimers
- **Channel styles** — per-channel tone and format rules (LinkedIn vs. Instagram vs. email)
- **Messaging frameworks** — approved key messages, taglines, positioning
- **Deliverable templates** — custom formats for reports, proposals, briefs (`/omni-growth-engine:import-template`)
- **Agency SOPs** — approval workflows, launch checklists, escalation procedures (`/omni-growth-engine:import-sop`)

Guidelines persist across sessions — import once, enforced every time. You can always add more later.

> See `docs/brand-guidelines.md` for the full guide with worked examples and all guideline categories.

---

## 6. Your First Marketing Task

Now that your brand profile is set up, try asking for some real marketing deliverables. You do not need to use any special commands --- just describe what you need in plain language.

### Example: Writing a welcome email sequence

```
You: Write a 3-email welcome sequence for new subscribers
```

Here is what happens behind the scenes when you make this request:

1. **Content Engine activates** --- the plugin recognizes this as an email marketing task
2. **Brand voice loads** --- your "warm, knowledgeable, passionate" voice profile shapes every word
3. **Compliance rules applied** --- CAN-SPAM requirements (unsubscribe link, physical address) are factored in
4. **Platform specs loaded** --- email best practices are applied (subject line under 40 characters for mobile, preview text 40-90 characters)

### What you receive

The plugin delivers a complete 3-email sequence, each with:

- **Subject line** (optimized for mobile preview length)
- **Preview text** (the snippet visible in inbox before opening)
- **Body copy** (written in your brand voice, structured for scannability)
- **Call to action** (clear, single CTA per email)

For Greenfield Coffee Roasters, the sequence would follow this arc:

| Email | Timing      | Theme                          | Voice emphasis  |
|-------|-------------|--------------------------------|-----------------|
| 1     | Immediate   | Welcome + brand story          | Warm            |
| 2     | Day 3       | Brewing guide + product rec    | Knowledgeable   |
| 3     | Day 7       | First purchase offer           | Passionate      |

- **Email 1** tells the Greenfield origin story, emphasizing sustainable sourcing and the people behind the beans. Tone is welcoming, like a friend inviting you into their world.
- **Email 2** shares a practical brewing guide (pour-over, French press, or AeroPress) and recommends a specific single-origin coffee to try. Tone is authoritative but approachable, sharing expertise without being preachy.
- **Email 3** extends a first-purchase offer with language that conveys genuine enthusiasm for quality. Tone is energetic and direct, with a clear CTA to shop.

### Other things to try

Here are a few more requests that work well as a first task:

- "Create a content calendar for next month" --- activates the Content Engine with your channels and content pillars
- "Audit my competitor Blue Bottle Coffee" --- activates Competitive Intelligence with industry benchmarks
- "Write Instagram captions for our new Ethiopian single-origin" --- activates Content Engine with Instagram platform specs
- "Build a buyer persona for our ideal customer" --- activates Audience Intelligence with your existing audience data
- "Plan a product launch for our new cold brew line" --- activates Campaign Orchestrator with your channels and budget context

Every response is automatically shaped by your brand profile. You never have to remind the plugin about your voice, audience, or compliance requirements.

### SEO Execution

Use `/omni-growth-engine:seo-implement` to update meta tags, deploy schema, and create redirects directly on WordPress or Webflow. `/omni-growth-engine:rank-monitor` sets up ongoing keyword tracking, and `/omni-growth-engine:rank-monitor --features` monitors SERP features including AI Overviews (the former `serp-tracker` skill merged into it).

For planning rather than execution: `/omni-growth-engine:keyword-cluster` turns seed keywords into a pillar-and-spokes content plan with SERP-overlap clustering and an internal-link map, `/omni-growth-engine:backlink-gap` finds domains linking to your competitors but not to you (ranked by a four-gate quality scorecard), and `/omni-growth-engine:seo-drift` compares two snapshots and classifies what moved — growth, decline, reshuffle, new, or lost. If you have Search Console access, `/omni-growth-engine:gsc-ai-performance` reads the AI Performance Report export so you can see AI Overviews and AI Mode impressions alongside classic search.

### Checking what's actually wired up

Not every action can run everywhere — some need credentials that only you can supply. `/omni-growth-engine:doctor` reports, per action, what is live versus blocked in your current environment and gives a one-step setup hint for anything blocked. When you are ready to fire a real API call rather than review a plan, `/omni-growth-engine:execute-action` does that: read operations run with `--execute`, write operations additionally require `--confirm`, and every execution is written to the audit trail.

### Competitor Monitoring

Use `/omni-growth-engine:competitor-monitor` to set up ongoing competitive scanning. `/omni-growth-engine:share-of-voice` calculates your visibility vs competitors. `/omni-growth-engine:competitor-alerts` configures notifications for competitive changes.

### Revenue Simulation

Use `/omni-growth-engine:simulate` to model revenue impact of budget changes with Monte Carlo simulation. `/omni-growth-engine:what-if` for quick scenario comparisons. `/omni-growth-engine:churn-risk` to score customer segments for churn probability.

### GEO Monitoring

Use `/omni-growth-engine:geo-monitor` to track brand visibility across ChatGPT, Perplexity, Gemini, and AI Overviews. `/omni-growth-engine:entity-audit` checks entity consistency across Wikidata, Knowledge Panel, and directories. `/omni-growth-engine:narrative-tracker` monitors what AI says about your brand.

### Creative Intelligence

Use `/omni-growth-engine:creative-health` for creative fatigue prediction across active ads. `/omni-growth-engine:content-decay-scan` finds decaying content and prioritizes refreshes by revenue impact.

### Synthetic Audiences

Use `/omni-growth-engine:focus-group` to run simulated focus groups from CRM data. `/omni-growth-engine:message-test` to pre-test messaging variants. `/omni-growth-engine:pricing-test` for price sensitivity analysis.

---

## Evaluation & Quality Assurance

### Quick Start
1. **Evaluate any content**: `/omni-growth-engine:eval-content` — runs the full 6-dimension eval suite
2. **Check for hallucinations**: Built into the 4 content-producer agents, which run a mandatory hallucination check before returning drafts. (You can also re-enable the reference Write|Edit hook to scan on every file save — it ships disabled; see `hooks/hooks-reference.example.json`.)
3. **Verify claims**: `/omni-growth-engine:verify-claims` with an evidence file for claims-heavy content
4. **Track quality over time**: `/omni-growth-engine:quality-report` for trends and regression alerts
5. **Configure thresholds**: `/omni-growth-engine:eval-config` to set brand-specific quality standards

### Evidence Files
For claim verification, create a JSON evidence file:
```json
{
  "evidence": [
    {"claim": "50% increase in conversions", "source": "GA4 Q4 report", "date": "2025-12-31", "verified": true}
  ]
}
```

### Eval Grades
A+ (95-100) through F (<40). Content below the auto-reject threshold (default 40) is blocked from the approval workflow.

---

## Multilingual Support

### Quick Start
1. **Configure languages**: `/omni-growth-engine:language-config` — set primary language, do-not-translate terms
2. **Translate content**: `/omni-growth-engine:translate-content` — auto-routes to best translation service
3. **Score translations**: `/omni-growth-engine:multilingual-score` — check quality before publishing
4. **Localize campaigns**: `/omni-growth-engine:localize-campaign` — adapt entire campaigns for target markets
5. **Audit hreflang**: `/omni-growth-engine:hreflang-check` — verify multilingual SEO implementation

### Translation Services Setup
Set environment variables for the services you want to use:
- **DeepL**: `DEEPL_API_KEY` — best for European and CJK languages
- **Sarvam AI**: `SARVAM_API_KEY` — specialist for 22 Indian languages
- **Google Cloud Translation**: `GOOGLE_APPLICATION_CREDENTIALS` — broadest coverage (100+ languages)
- **Lara Translate**: `LARA_API_KEY` — marketing-context translation with translation memories

### Language Configuration
Run `/omni-growth-engine:language-config` to set:
- Primary content language
- Target languages for translation
- Do-not-translate terms (brand names, product names)
- Translation service preferences per language

---

## 7. Your First Full Engagement (v3.0)

Sections 1–6 cover the v2.x way of using the plugin: one-off tasks driven by `/omni-growth-engine:` commands. Both paths are supported and useful.

For a real strategic engagement (a quarterly strategy, an annual plan, a new client onboarding, a major repositioning), v3.0 adds the **12-Part Engagement Methodology**. It orchestrates the same agents, skills, and connectors into a sequential workflow that produces ~50–60 traceable files per engagement.

### When to use the engagement workflow vs one-off commands

| You need | Use |
|---|---|
| A single deliverable (one campaign, one email sequence, one audit) | One-off `/omni-growth-engine:` commands |
| A full strategic engagement with traceable rationale | `/omni-growth-engine:engagement` workflow |
| Quick exploration | One-off commands |
| Client-presentable Growth Plan + Yearly Planner | `/omni-growth-engine:engagement` workflow |
| Internal team experimentation | One-off commands |
| Multi-month engagement that needs version control + Update-Back corrections | `/omni-growth-engine:engagement` workflow |

### Running an engagement — the short version

```bash
# One-time per brand (already covered above)
/omni-growth-engine:brand-setup

# Per engagement
/omni-growth-engine:engagement start <brand-slug> <engagement-id>     # Init + Part 1 intake
/omni-growth-engine:engagement four-core <brand> <id>                 # Produces Part 3 (61 steps across 4 docs)
/omni-growth-engine:engagement validate <brand> <id>                  # Part 5 client validation document
/omni-growth-engine:engagement re-run-decision <brand> <id>           # Part 6 v2 re-runs per Decision Matrix
/omni-growth-engine:engagement growth-plan <brand> <id>               # Part 8 flagship deliverable
/omni-growth-engine:engagement yearly-planner <brand> <id>            # Part 8 operational companion
/omni-growth-engine:engagement loop <brand> <id>                      # Part 12 continuous improvement
/omni-growth-engine:engagement status                                 # Check progress at any time
```

### What the methodology adds

- **Stone vs Opinion intake** — separates verifiable client facts from client beliefs (which become research questions, not ground truth)
- **Two-Views Model** — keeps both v1 (unbiased market view) and v2 (client-validated view) available forever
- **Decision Matrix** — selectively re-runs only the documents affected by client validation; prevents over- and under-re-running
- **Update-Back Rule** — versioning protocol for in-life corrections (v2.1, v2.2 ...) so the strategy stays honest over time
- **Living Project Instruction File** — single source of truth that all skills read first

### What you get out

Per engagement, a structured directory at `~/.claude-marketing/brands/{brand-slug}/engagements/{engagement-id}/`:

```
├── _engagement.json                  # State + version history
├── living-instruction-file.md        # "Currently true" record
├── part-01-client-inputs/            # Stone facts + Opinion hypotheses
├── part-02-external-research/        # 3 unbiased research docs
├── part-03-four-core-documents/      # v1/ + v2/ (Four Core Documents, 61 steps)
├── part-04-competitive-customer-market/  # v1/ + v2/ (4 docs)
├── part-05-client-validation/        # The "one true stop"
├── part-06-v2-reruns/                # Decision Matrix log
├── part-07-preparation/              # 6 internal operating docs
├── part-08-growth-plan/              # Growth Plan + Yearly Planner (client-facing)
├── part-09-channel-strategy/         # Up to 17 channel docs
├── part-10-execution-artefacts/      # Ad copy, post copy, headlines, CTAs
├── part-11-ai-creative-instructions/ # Visual asset briefs
├── part-12-continuous-improvement/   # Quarterly + ad-hoc briefs
└── reports/                          # monthly/ quarterly/ annual/
```

Out of those ~50–60 files, only Parts 5 and 8 produce client-facing deliverables — the rest are internal operating documents that prioritise depth, rationale, and assumption discipline.

### Where to learn more

The full user-facing methodology guide is at **[docs/engagement-methodology.md](engagement-methodology.md)**. It covers:

- Why a methodology
- The 12 parts in detail
- The Two-Views Model
- The Decision Matrix
- The Update-Back Rule
- The Living Project Instruction File
- Reading the engagement directory
- Quality discipline
- Common patterns and anti-patterns

The methodology is supported by 23 reference documents in `skills/context-engine/`. The most foundational ones to read first:

- [engagement-flow-methodology.md](../skills/context-engine/engagement-flow-methodology.md) — the master 12-Part flow specification
- [four-core-documents-spec.md](../skills/context-engine/four-core-documents-spec.md) — the 61-step specification for Part 3
- [stone-vs-opinion.md](../skills/context-engine/stone-vs-opinion.md) — confidence tagging at intake
- [two-views-model.md](../skills/context-engine/two-views-model.md) — v1 + v2 architecture

---

## 8. Understanding the Session Lifecycle

OmniGrowth Engine can operate across three lifecycle phases in a Claude Code or Cowork session. Understanding this lifecycle helps you get the most out of the plugin.

> **Note (v3.1+):** These three lifecycle hooks ship **disabled** (`hooks/hooks.json` is empty) so the plugin never interferes with non-marketing work in other projects. The reference configuration is preserved in `hooks/hooks-reference.example.json` — copy the entries you want back into `hooks/hooks.json` to get the automatic behavior described below. With hooks off, the same outcomes are available on demand: brand context loads when a skill runs (or via `/omni-growth-engine:status`), content is checked by the content-producer agents' built-in hallucination gate, and you save insights with `/omni-growth-engine:sync-memory`.

### Phase 1: Session Start

**What happens:** When enabled, the SessionStart hook fires and loads your active brand context into the session.

**What this means for you:** From the very first message you type, Claude already knows your brand name, voice settings, industry, compliance requirements, target audience, active channels, competitors, and current goals. You never have to re-explain who you are or what your brand sounds like.

**What you see:** The 15-line brand summary banner printed at the top of your session.

### Phase 2: During Your Session

**What happens:** As you make requests, the appropriate marketing modules activate automatically. Three things are applied to every piece of marketing output:

- **Brand voice** --- content matches your formality, energy, humor, and authority settings
- **Compliance rules** --- outputs respect regulations for your industry and target markets (GDPR, FTC, HIPAA, and others)
- **Industry benchmarks** --- recommendations are calibrated to realistic performance standards for your sector

**The PreToolUse hook:** When the plugin writes content to a file, the PreToolUse hook can check it for brand alignment and compliance before saving. This acts as a guardrail to catch anything that drifts off-brand.

**What you see:** Marketing deliverables that sound like they came from someone who has worked with your brand for months, not minutes.

### Phase 3: Session End

**What happens:** When enabled, the SessionEnd hook fires and saves key marketing insights from the session to your brand profile. This includes things like:

- New audience insights discovered during persona research
- Competitor intelligence gathered during analysis
- Campaign decisions and strategic direction
- Content performance hypotheses

**What this means for you:** The next time you start a session, those insights are already part of your brand context. The plugin gets smarter about your brand over time, building a growing body of institutional marketing knowledge.

**What you see:** A brief confirmation that insights have been saved.

### The lifecycle in one diagram

```
Session Start              During Session             Session End
     |                          |                          |
     v                          v                          v
Brand context            Modules activate           Insights saved
auto-loaded         Voice + Compliance + Benchmarks    to brand profile
     |               applied to all outputs              |
     v                          |                        v
15-line summary          You work normally         Ready for next
printed                  (just ask for things)        session
```

---

## 9. Python Dependencies (Optional)

OmniGrowth Engine is designed to work at full capability without Python. All 24 specialist agents, 18 top-level commands, and 163 skills function using the plugin's built-in reference knowledge (169 reference files). Python adds bonus scoring and automation features (and the engagement-state, dm-status, auto-save-insight, and eval scripts that power the v3.0 methodology + v3.2 quality gates — install Python if you plan to use the engagement workflow or the /omni-growth-engine:check + /omni-growth-engine:status commands).

### Three dependency modes

| Mode               | Install size | What it adds                                                       |
|--------------------|--------------|--------------------------------------------------------------------|
| **Knowledge-only** | 0 MB         | All modules, agents, and commands. No Python needed.               |
| **Lite**           | ~15 MB       | Brand voice scoring, content quality scoring, readability analysis  |
| **Full**           | ~50 MB       | Competitor scraping, QR code generation, AI visibility checking     |

### Knowledge-only (default)

This is what you get out of the box. No setup required.

You have access to:
- All 16 marketing modules with 169 reference knowledge files (including the v3.0 methodology references)
- All 163 skills + 18 top-level `/omni-growth-engine:` commands (including the v3.0 engagement workflow)
- All 24 specialist agents
- Brand profiling and campaign tracking (session hooks are opt-in — see the Session Lifecycle section)
- Industry benchmarks, compliance rules, and platform specifications
- The 12-Part engagement methodology (the engagement-state script requires Python — see below)

The plugin will tell you when a Python-dependent feature is unavailable and will gracefully skip it rather than throwing an error.

### Lite mode

If you want brand voice scoring and content readability analysis, install two small packages:

```
pip install nltk textstat
```

This unlocks:
- **Brand voice scoring** --- quantitative alignment score (0-100) measuring how well content matches your voice profile
- **Content quality scoring** --- readability grade level, sentence complexity, and vocabulary analysis
- **Readability analysis** --- Flesch-Kincaid, Gunning Fog, and other standard readability metrics

### Full mode

For the complete feature set, install all dependencies:

```
pip install -r /path/to/omni-growth-engine/scripts/requirements.txt
```

This adds everything in Lite mode, plus:
- **Competitor scraping** --- automated extraction of competitor page titles, meta descriptions, and content structure
- **QR code generation** --- create QR codes for UTM-tagged URLs (useful for print-to-digital campaigns)
- **AI visibility checking** --- programmatic checks of how your brand appears in AI answer engines (requires OpenAI or Anthropic API key in `.env`)

### How to check your current mode

The brand summary banner (from `/omni-growth-engine:status`, or at session start if you enabled the SessionStart hook) includes a Python status line:

```
Python: not installed          (knowledge-only mode)
Python: lite (nltk, textstat)  (lite mode)
Python: full (all deps)        (full mode)
```

---

## 10. Connector Discovery

OmniGrowth Engine includes a connector discovery system that makes it easy to see which external platforms are connected and set up new ones.

### Checking your connector status

```
/omni-growth-engine:integrations
```

This shows a dashboard grouped by category (chat, design, CRM, SEO, advertising, analytics, and more) with each connector marked as **connected** or **available**. It also shows which skills gain capabilities from each connector.

Example output:

```
=== CONNECTOR STATUS ===

 Chat                           Connected
  slack                         ✅ HTTP
  intercom                      ○ npx (needs INTERCOM_ACCESS_TOKEN)

 Design                         Connected
  canva                         ✅ HTTP
  figma                         ✅ HTTP

 CRM                            Partial
  hubspot                       ✅ HTTP
  salesforce                    ○ npx (needs SALESFORCE_INSTANCE_URL, SALESFORCE_ACCESS_TOKEN)
  pipedrive                     ○ npx (needs PIPEDRIVE_API_TOKEN)

 ...

Connected: 10 HTTP | Available: 68-server catalog
Skills fully unlocked: 87/158 | Skills with enhanced capabilities: **158/158**
```

### Setting up a new connector

```
/omni-growth-engine:connect slack
```

For HTTP connectors (like Slack, Canva, HubSpot), you get OAuth-based setup instructions that work in both Cowork and Claude Code. For npx connectors (like Salesforce, Google Ads), you get step-by-step credential setup instructions.

### Platform-level integrations

Some integrations (like Google Drive and Google Sheets) may be connected at the Claude platform level rather than through MCP. These platform-level integrations are managed in Claude Desktop settings and work automatically in Cowork sessions. The plugin can use these integrations even if they do not appear in the connector status dashboard.

To check platform-level integrations: Open Claude Desktop → Settings → Integrations.

---

## 11. Available Commands

OmniGrowth Engine provides 163 skills + 18 top-level commands, all prefixed with `/omni-growth-engine:`. You can type these directly in your Claude Code session.

### Pre-Publish Quality + Status (v3.2)

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:check <file-or-content>` | Quick eval (~2s): hallucination + content quality + readability |
| `/omni-growth-engine:check <file> --full --brand <slug>` | Full 6-dimension eval including brand voice + claims + structure |
| `/omni-growth-engine:check <file> --compliance --brand <slug> --evidence <facts.json> --schema <name>` | Compliance-focused eval for regulated industries |
| `/omni-growth-engine:status` | Unified snapshot: brand profile + engagements + insights + compliance + deps |
| `/omni-growth-engine:status --quiet` | One-line compact summary |
| `/omni-growth-engine:status --json` | Machine-readable JSON for downstream skills |
| `/omni-growth-engine:status --section <brand\|engagements\|insights\|compliance\|deps>` | Single section only |

### Engagement Workflow (v3.0)

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:engagement start <brand> <id>` | Initialise a new engagement; walks Part 1 Stone vs Opinion intake |
| `/omni-growth-engine:engagement status [brand] [id]` | Show current engagement status (or list all if omitted) |
| `/omni-growth-engine:engagement next` | Advance to next part after confirming current is complete |
| `/omni-growth-engine:engagement four-core <brand> <id>` | Produce Part 3 Four Core Documents (61 steps); supports `--doc 3.X` and `--view v2` |
| `/omni-growth-engine:engagement validate <brand> <id>` | Produce Part 5 Client Validation Document (the "one true stop") |
| `/omni-growth-engine:engagement re-run-decision <brand> <id>` | Apply Decision Matrix to determine v2 re-runs |
| `/omni-growth-engine:engagement growth-plan <brand> <id>` | Produce Part 8 flagship 11-section client deliverable |
| `/omni-growth-engine:engagement yearly-planner <brand> <id>` | Produce Part 8 12-month operational companion |
| `/omni-growth-engine:engagement loop <brand> <id>` | Produce Part 12 quarterly or ad-hoc continuous improvement brief |
| `/omni-growth-engine:engagement update-back <brand> <id> --doc <X> --reason "..."` | Bump source document version per the Update-Back Rule |
| `/omni-growth-engine:engagement lif-show <brand> <id>` | Display the Living Project Instruction File |
| `/omni-growth-engine:engagement file-tree <brand> <id>` | Show the engagement directory file tree |
| `/omni-growth-engine:engagement list-engagements [brand]` | List all engagements (optionally filter by brand) |

### Brand Management

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:brand-setup` | Create or update a brand profile through interactive guided setup |
| `/omni-growth-engine:switch-brand` | Switch the active brand for multi-client and agency workflows |

### Strategy and Planning

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:campaign-plan` | Build a multi-channel campaign plan with objectives, targeting, budget, and KPIs |
| `/omni-growth-engine:launch-plan` | Create a product or feature launch playbook across pre-launch, launch day, and post-launch phases |
| `/omni-growth-engine:social-strategy` | Develop a platform-specific social media strategy with content pillars and growth plan |
| `/omni-growth-engine:competitor-analysis` | Run a multi-dimensional competitive analysis covering content, SEO, ads, social, and positioning |
| `/omni-growth-engine:media-plan` | Holistic paid media planning with channel allocation, flight scheduling, and creative rotation |
| `/omni-growth-engine:client-onboarding` | Post-sale client onboarding workflow with kickoff agenda, discovery questionnaire, and 30-60-90 plan |
| `/omni-growth-engine:qbr-plan` | Quarterly Business Review preparation with performance retrospective and strategic recommendations |

### Content Creation

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:content-brief` | Generate a detailed content brief with keyword targets, outline, and SEO requirements |
| `/omni-growth-engine:content-calendar` | Build a monthly or quarterly content calendar with platform assignments and repurposing workflows |
| `/omni-growth-engine:email-sequence` | Create a complete email sequence with subject lines, body copy, timing, and segmentation |
| `/omni-growth-engine:ad-creative` | Produce platform-specific ad copy variations with quality scoring for Google, Meta, LinkedIn, and TikTok |
| `/omni-growth-engine:video-script` | Video marketing script writing for YouTube, TikTok, Reels, and LinkedIn with hooks and timestamps |
| `/omni-growth-engine:case-study-plan` | Structured case study creation with CSR framework, interview questions, and distribution strategy |

### Analysis and Audits

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:seo-audit` | Run a comprehensive SEO audit covering technical health, on-page, content, E-E-A-T, and links |
| `/omni-growth-engine:tech-seo-audit` | Technical SEO audit: Core Web Vitals, crawlability, indexation, redirects, site architecture, security |
| `/omni-growth-engine:local-seo-audit` | Local SEO audit: Google Business Profile, NAP consistency, citations, local pack, reviews |
| `/omni-growth-engine:aeo-audit` | Assess how your brand appears in AI-powered search and answer engines (ChatGPT, Perplexity, Google AI Overviews) |
| `/omni-growth-engine:landing-page-audit` | Score a landing page across above-fold clarity, trust signals, form friction, and mobile experience |
| `/omni-growth-engine:funnel-audit` | Analyze your customer funnel for drop-off points, bottlenecks, and optimization opportunities |
| `/omni-growth-engine:performance-report` | Generate a marketing performance report with KPI tracking, trend analysis, and recommendations |

### Outreach and PR

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:pr-pitch` | Create media pitch packages with templates, target media lists, and outreach strategy |
| `/omni-growth-engine:influencer-brief` | Build an influencer campaign brief with discovery criteria, creator guidelines, and FTC compliance |
| `/omni-growth-engine:crisis-response` | Get rapid crisis assessment with severity scoring, stakeholder messaging, and communication timeline |

### Audience

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:audience-profile` | Build a detailed buyer persona with demographics, psychographics, behaviors, and content preferences |

### Data & Optimization

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:keyword-research` | Guided keyword research with clustering, intent mapping, and content gap analysis |
| `/omni-growth-engine:roi-calculator` | Calculate campaign ROI with 5 attribution models and budget efficiency ranking |
| `/omni-growth-engine:ab-test-plan` | Plan A/B tests with hypothesis framework, sample size calculation, and duration estimation |
| `/omni-growth-engine:content-repurpose` | Generate content repurposing strategy with derivative format matrix and publishing calendar |
| `/omni-growth-engine:retargeting-strategy` | Build retargeting campaign architecture with audience segmentation and frequency capping |
| `/omni-growth-engine:martech-audit` | Audit marketing technology stack across 11 functions with overlap detection and gap analysis |
| `/omni-growth-engine:budget-optimizer` | Data-driven budget reallocation with diminishing returns modeling and efficiency ranking |
| `/omni-growth-engine:attribution-model` | Multi-touch attribution setup with model selection and credit distribution rules |
| `/omni-growth-engine:creative-testing-framework` | Systematic creative testing strategy with testing matrix and holdout controls |
| `/omni-growth-engine:executive-dashboard` | C-suite dashboard design with business-outcome metrics and alert thresholds |
| `/omni-growth-engine:client-proposal` | Generate agency client proposal with situation analysis, strategy, scope, and pricing |
| `/omni-growth-engine:review-response` | Draft brand-aligned review responses with tone templates and escalation detection |
| `/omni-growth-engine:webinar-plan` | End-to-end webinar planning with promotion timeline, email sequences, and post-event nurture |

### Execution & Publishing

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:publish-blog` | Publish blog post to WordPress/Webflow with SEO metadata and scheduling |
| `/omni-growth-engine:send-email-campaign` | Send email campaign via SendGrid/Klaviyo/Brevo with personalization and A/B testing |
| `/omni-growth-engine:launch-ad-campaign` | Create paid ad campaign on Google/Meta/LinkedIn/TikTok with budget safeguards |
| `/omni-growth-engine:schedule-social` | Schedule posts to Twitter/Instagram/LinkedIn/TikTok/YouTube/Pinterest |
| `/omni-growth-engine:send-report` | Generate and deliver performance report via Slack, email, or Sheets |

### CRM & Data

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:crm-sync` | Sync marketing contacts and deals to Salesforce/HubSpot/Zoho/Pipedrive |
| `/omni-growth-engine:lead-import` | Import leads from forms, CSV, or manual entry into CRM with deduplication |
| `/omni-growth-engine:pipeline-update` | Update deal stages, values, and notes in CRM pipeline |
| `/omni-growth-engine:segment-audience` | Create or update audience segments in CRM or email platform |
| `/omni-growth-engine:data-export` | Export marketing data to BigQuery, Google Sheets, or Supabase |

### Monitoring

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:performance-check` | Pull live metrics from all connected platforms for instant performance snapshot |
| `/omni-growth-engine:campaign-status` | Check status of all active campaigns with execution history |
| `/omni-growth-engine:anomaly-scan` | Detect anomalies --- traffic drops, spend spikes, deliverability issues |
| `/omni-growth-engine:budget-tracker` | Real-time budget tracking across all ad platforms with pacing analysis |

### Memory & Knowledge

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:save-knowledge` | Save brand knowledge to vector database for RAG retrieval |
| `/omni-growth-engine:search-knowledge` | Semantic search across all stored brand knowledge |
| `/omni-growth-engine:sync-memory` | Batch sync session learnings and campaign history to persistent memory |

### Communication

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:send-sms` | Send SMS or WhatsApp marketing message via Twilio or Brevo |
| `/omni-growth-engine:send-notification` | Send team notification via Slack with campaign updates or alerts |

### Agency Operations

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:agency-dashboard` | Portfolio-level view across all clients with KPI health and budget pacing |
| `/omni-growth-engine:client-report` | Generate white-labeled client-facing performance report |
| `/omni-growth-engine:sop-library` | Manage agency SOPs --- create, assign to brands, track compliance |
| `/omni-growth-engine:credential-switch` | Switch active brand credential profile for multi-client management |

### Brand Team Management

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:team-assign` | Assign marketing tasks to team members based on role and capacity |
| `/omni-growth-engine:region-config` | Configure regional settings --- timezone, language, compliance, currency |
| `/omni-growth-engine:exec-summary` | Generate C-suite executive summary with portfolio ROI and strategic recommendations |

### Connector Discovery

| Command | What it does |
|---------|-------------|
| `/omni-growth-engine:integrations` | See which connectors are active, which are available, and what skills each unlocks |
| `/omni-growth-engine:connect <name>` | Step-by-step setup guide for any connector (HTTP or npx) |

### Tip: You do not always need slash commands

Slash commands are useful for structured, templated outputs. But you can also just describe what you need in natural language:

```
"Help me write ad copy for our new cold brew"
"What should our content strategy look like for Q3?"
"I need to respond to negative reviews on Google"
```

The plugin's skills will activate based on the intent of your request, whether or not you use a slash command. The 163 skills + 18 top commands simply give you a direct shortcut to a specific workflow.

---

## 12. Next Steps

You are set up and ready to go. Here are some resources for when you want to go deeper.

### Guides

- **The 12-Part Engagement Methodology (v3.0)** --- For full strategic engagements with traceable rationale and version-controlled deliverables, see [docs/engagement-methodology.md](engagement-methodology.md). This is the higher-leverage way to use the plugin.

- **Importing brand guidelines** --- If your brand has a voice guide, restriction list, or channel-specific style rules, see [docs/brand-guidelines.md](brand-guidelines.md) for the full guide on importing guidelines, templates, and agency SOPs.

- **Managing multiple brands** --- If you work with more than one brand or run an agency, see [docs/multi-brand-guide.md](multi-brand-guide.md) for brand switching, side-by-side comparison, multi-client workflows, and per-brand engagement isolation.

- **Execution & Publishing** --- v2.0.0+ adds full execution capabilities. Every action goes through an approval workflow (draft → review → approve → execute → monitor). See the execution commands above.

- **CRM Integration** --- Connect Salesforce, HubSpot, Zoho, or Pipedrive for bidirectional data sync. See [docs/integrations-guide.md](integrations-guide.md) for setup.

- **Memory & RAG** --- Store and retrieve brand knowledge across sessions using Pinecone, Qdrant, or Supermemory vector databases. See the Memory & Knowledge commands above.

- **Connecting your marketing tools** --- The plugin supports 10 registry-backed HTTP MCP connectors + a 68-server opt-in catalog spanning analytics, advertising, CRM, email, social publishing, memory/RAG, and more. See [docs/integrations-guide.md](integrations-guide.md) to connect your accounts.

- **KPI-driven strategy** --- Learn how to set up marketing KPI frameworks, build reporting dashboards, and track campaign performance over time in [docs/strategy-and-kpis.md](strategy-and-kpis.md).

- **Understanding the architecture** --- For a technical deep dive into how the 163 skills, 24 agents, context engine, hook system, and v3.0 methodology layer work together, see [docs/architecture.md](architecture.md).

- **Using Cowork** --- If you are using Claude Cowork (or considering it), see [docs/claude-interfaces.md](claude-interfaces.md) for Cowork-specific capabilities like document creation, visual page review, and a comparison with other marketing plugins.

### Quick reference: The 16 marketing modules

These are the knowledge domains that power the plugin. They activate automatically based on your requests.

| Module                  | Coverage                                                            |
|-------------------------|---------------------------------------------------------------------|
| Content Engine          | SEO content, ad copy, email, social, landing pages, brand voice, accessibility, multilingual |
| Campaign Orchestrator   | Campaign planning, budget allocation, channel strategy, UTM tracking, post-mortems, ABM |
| Audience Intelligence   | Buyer personas, segmentation, Jobs-to-Be-Done, psychographic profiling |
| Analytics & Insights    | KPI frameworks, reporting, anomaly diagnosis, competitive intel, attribution, MMM |
| Paid Advertising        | Google Ads, Meta Ads, LinkedIn Ads, TikTok Ads, programmatic, retail media, bid strategy |
| AEO/GEO                | AI visibility, answer engine optimization, citation optimization, entity consistency |
| Funnel Architect        | Journey mapping, funnel design, attribution models, gap analysis    |
| CRO                     | Landing page audits, A/B testing, form optimization, pricing psychology, checkout optimization |
| Digital PR              | Media outreach, press releases, thought leadership, newsjacking, E-E-A-T authority |
| Growth Engineering      | Product-led growth, referral systems, viral loops, launch strategy, retention, affiliate |
| Influencer & Creator    | Influencer discovery, creator briefs, FTC compliance, contracts, UGC, performance tracking |
| Reputation Management   | Review strategy, crisis communication, brand safety, sentiment monitoring, recovery playbooks |
| Emerging Channels       | Voice search, visual search, conversational commerce, social commerce, podcasts, video, community |
| Technical SEO           | Core Web Vitals, crawlability, indexation, site architecture, redirects, JavaScript SEO, mobile-first |
| Local SEO               | Google Business Profile, NAP consistency, citations, local pack, location pages, multi-location |
| Marketing Automation    | Automation workflows, lead scoring, nurture sequences, marketing operations, MAP strategy |

### Getting help

If something is not working as expected:

1. Check that your brand profile exists: look for a file at `~/.claude-marketing/brands/_active-brand.json`
2. Re-run brand setup if needed: `/omni-growth-engine:brand-setup`
3. Check Python status in your session start banner (if you expected scoring features)
4. For MCP integration issues, verify your API credentials in the `.mcp.json` configuration

---

*OmniGrowth Engine v3.17.0 --- Built for marketing professionals who want strategy, execution, and publishing that stays on-brand, every time. v3.0 added the 12-Part Engagement Methodology with traceable rationale, version-controlled deliverables, and the Two-Views Model. v3.2 adds /omni-growth-engine:check (pre-publish gate), /omni-growth-engine:status (on-demand snapshot), and embedded mandatory hallucination checks in 4 content-producer agents — closing the gaps from the v3.1 multi-plugin hook removal. Plan it, approve it, execute it, monitor it --- all from Claude Code and Claude Cowork. Built by [Indranil Banerjee](https://github.com/swastik-agnihotri).*
