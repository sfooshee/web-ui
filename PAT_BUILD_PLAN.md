# PAT — Build Plan & Development Handoff
> Living document. Updated through founder interview. Last updated: 2026-04-22.

---

## STATUS: IN PROGRESS — Interview ongoing. Sections marked 🔲 are incomplete.

---

## 1. What We're Building

Three deliverables:
1. **Marketing website** — motion-graphics-led, Pixar/Toy Story aesthetic, character-driven
2. **Web app** — Pat Chat interface, dashboards, workflow management, client onboarding
3. **Per-client mini-AGI architecture** — every client gets their own Pat instance with isolated memory, tools, and policy knowledge

---

## 2. The Problem

Healthcare providers collect ~48% of patient balances. Bad debt is driven by:
- Insured patients (not uninsured — the assumption is wrong)
- Inconsistent staff collection behavior
- No real-time policy enforcement at point of care

---

## 3. The Product — What Pat Is

> Pat is not a calculator. Pat is an agent with a toolbelt.

Each client gets a dedicated Pat instance — a character-driven AI agent with:
- **EMR tools** — read appointments, patient records, write back decisions (via MCP, supergood.ai)
- **Insurance tools** — real-time eligibility and benefits verification via API
- **Email tools** — send reports, alerts, patient communications
- **Knowledge base** — the client's own financial policy + a compliant base template provided by PAT the company
- **Workflow library** — pre-built and custom workflows stored as `.md` files, activatable/deactivatable by the client

The main interface is **Pat Chat** — a conversational UI where staff interact with Pat directly. Supporting surfaces include dashboards (activity feed, performance metrics) and a Workflows tab.

---

## 4. Onboarding Flow

1. Client signs up and "meets Pat"
2. Pat introduces himself and explains his toolbelt
3. Pat walks the client through connecting tools:
   - **EMR integration**: MCP connector (via supergood.ai) or manual CSV/data upload
   - **Insurance verification**: API key setup (vendor TBD)
   - **Email**: outbound configuration
4. Pat helps the client define their first workflow together (conversational setup)
5. Client sees their workflow in the Workflows tab, activates it
6. Pat runs it — and reports back

---

## 5. Core Workflows (Library)

Workflows are stored as `.md` files per client. Each workflow defines:
- Trigger (schedule, event, manual)
- Steps (tool calls + logic)
- Output (report, EMR write-back, email, dashboard entry)

**Example workflow: Pre-visit insurance sweep**
- Trigger: Nightly, 48hrs before appointment
- Steps: Pull upcoming appointments → verify insurance for each patient → flag changes or gaps
- Output: Email report to designated staff + write flags back to EMR

🔲 Full workflow library TBD (based on interview)

---

## 6. Policy Engine

- Each client Pat has a **collections policy** — stored as structured config/code
- Base template provided by PAT (compliant defaults)
- Client customizes via Pat Chat ("Pat, update our policy — we want to always offer a payment plan for balances over $500")
- Policy is versioned, auditable, and stored as code
- Pat applies policy when generating collection recommendations

Policy inputs:
- Collection goal (maximize collections vs. maximize access)
- Minimum upfront amount
- Balance thresholds for payment plans
- Payer/plan-specific exceptions
- Patient segment rules

🔲 Policy schema: TBD

---

## 7. Branding & Design Direction

### Character
- Name: **Pat**
- Tagline: **"Your Financial Handyman"**
- Visual: Pixar-style 3D character — blue work shirt, rolled sleeves, name tag, toolbelt loaded with tools
- Callsign: **thumbs up** 👍
- Personality: Whimsical but competent. Fixes confusion, builds solutions, cares about people.

### Website Visual Direction
- Motion-graphics-led (not static)
- Animations show Pat physically interacting with an EMR:
  - Pat running through the system
  - Unscrewing a field nameplate, replacing it
  - Pulling tools from his belt to fix problems
- Aesthetic: Toy Story / Pixar — warm, handcrafted-feeling 3D animation
- Feel: A little guy who shows up and gets to work

### Color / Tone
🔲 Full palette TBD — starting point from character: navy/denim blue, warm tan/khaki, brown leather (toolbelt), warm white bg

---

## 8. Technical Architecture

### Guiding Constraint
> The founder is non-technical. A multi-agent coding team manages all infrastructure. Every major system component must be operable via API, CLI, or MCP — no GUI-only admin tools.

### Per-Client Pat Architecture (Mini-AGI)
Each client gets an isolated Pat instance:

| Layer | Component | Notes |
|-------|-----------|-------|
| Agent core | LLM (Claude) | Per-client system prompt + policy context injected |
| Memory | Vector DB per client | Stores policy, past decisions, workflow history |
| Tools | EMR (MCP via supergood.ai) | Read/write to client's EMR |
| Tools | Insurance API | Eligibility + benefits verification |
| Tools | Email | Outbound notifications and reports |
| Tools | Policy engine | Apply client's financial rules |
| Config | Workflow library | `.md` files, stored per client, version-controlled |
| Config | Policy config | Structured JSON/YAML, versioned |

🔲 Isolation model (shared infra, per-client data isolation vs. fully separate stacks) — TBD
🔲 Memory architecture (vector DB vendor, embedding model) — TBD
🔲 Workflow execution engine (how `.md` workflows become runnable jobs) — TBD
🔲 LLM orchestration layer (LangGraph? custom? Claude tool-use native?) — TBD

### Backend
- Headless-first
- Every action exposed via REST API
- CLI surface for: policy versioning, simulation, deployment, testing, connector setup
- MCP surface for coding agents to inspect and operate the system

🔲 Language/framework — TBD
🔲 Database — TBD
🔲 Auth/multi-tenancy model — TBD
🔲 HIPAA compliance posture — TBD

### Frontend (Web App)
- Pat Chat (primary interface)
- Dashboard: activity feed, performance metrics
- Workflows tab: list of client's workflows, activate/deactivate toggle
- Onboarding flow (guided, character-led)
- Policy viewer/editor (simple, staff-friendly)

🔲 Frontend framework — TBD
🔲 Chat UI approach — TBD

### Marketing Website
- Motion graphics / animation-heavy
- Pixar-style Pat character animations
- Key scenes to animate:
  - Pat jumping into an EMR
  - Replacing a field nameplate
  - Running through a patient list with his clipboard
- Static hosting preferred (fast, cheap, agent-manageable)

🔲 Animation approach (Lottie? Three.js? CSS? Video loops?) — TBD
🔲 Hosting — TBD

---

## 9. Integrations

| Integration | Purpose | Vendor | Status |
|-------------|---------|--------|--------|
| EMR (MCP) | Read/write patient + appointment data | supergood.ai | 🔲 TBD |
| Insurance eligibility | Real-time benefits verification | 🔲 TBD | 🔲 TBD |
| Email | Outbound reports + alerts | 🔲 TBD | 🔲 TBD |
| Payment processing | Payment plan execution | 🔲 TBD — own vs. integrate | 🔲 TBD |

---

## 10. Business Model

🔲 Pricing model — TBD
🔲 Target customer profile — TBD
🔲 Go-to-market — TBD
🔲 First customer / pilot — TBD

---

## 11. MVP Scope

🔲 To be defined — pending interview

---

## 12. Build Phases

🔲 To be defined — pending interview

---

## 13. Open Questions (Active)

- [ ] Business model: SaaS? Per-client fee? Usage-based?
- [ ] First target customer: specialty type, size, geography?
- [ ] MVP: what's the minimum Pat needs to do to be valuable on day 1?
- [ ] Per-client Pat isolation: shared infra + data isolation, or fully separate?
- [ ] LLM orchestration: Claude tool-use native, LangGraph, other?
- [ ] Eligibility API vendor choice
- [ ] Payment processing: own rails or integrate (Stripe, etc.)?
- [ ] HIPAA compliance: BAA required on day 1?
- [ ] Onboarding: self-serve or white-glove?
- [ ] Team: who else is building this?
- [ ] Timeline / funding situation

---

*This document is maintained through a live founder interview. Each section will be finalized before being handed to development agents.*
