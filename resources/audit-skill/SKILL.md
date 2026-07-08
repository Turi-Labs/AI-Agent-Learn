---
name: client-audit
description: Turn raw client conversations and your honest read of a business into a professional, ROI-ranked audit and execution roadmap. Use when the user wants to create a client audit, business audit, ROI-ranked roadmap, or audit report from call notes, transcripts, or conversation dumps. Triggers: client audit, business audit, run an audit, audit this client, ROI roadmap.
---

# Client Audit Skill

An audit is not a writing task. It's a judgement task.

Any model can format a document. Clients pay for the judgement: what's broken, what it's costing them, and what to fix first. This skill turns raw material — conversations, notes, your honest read — into an audit that ranks every problem by ROI and hands back a roadmap.

Best run on the strongest model available (judgement-heavy). The output roadmap is designed to be executed by cheaper models over months. Judge once, execute forever.

## Inputs

Ask for (or locate in the working directory) three things:

1. **Raw conversations** — call transcripts, WhatsApp/email threads, meeting notes with the client. Unedited. More is better.
2. **The operator's raw read** — the user's honest, unpolished take on the business: what they suspect is broken, what the client won't admit, gut feel included. Never skip this. The audit's judgement should align with and sharpen the operator's judgement, not replace it.
3. **Business context** — industry, team size, rough revenue if known, what the client already tried.

If only conversations are provided, ask for the raw read before writing anything. It is the highest-signal input.

## Process

### Step 1 — Extract problems, not topics

Read everything. List every operational problem the material reveals — stated or implied. For each, note the evidence (quote or paraphrase from the source material). Discard anything with no evidence; an audit built on assumptions gets torn apart on the client call.

### Step 2 — Cost each problem

For every problem, estimate what it costs the business:

- **Hours/week** lost (admin, follow-ups, quoting, reporting, rework)
- **Revenue leaked** (missed follow-ups, slow quotes, churn)
- **Risk carried** (single point of failure, compliance, key-person dependency)

Use the client's own numbers where the conversations contain them. Where you estimate, say so and show the assumption. A defensible rough number beats a precise invented one.

### Step 3 — Rank by ROI

Score each problem: `ROI = (cost of problem × fixability) / effort to fix`. Sort descending. The top 3 are the audit's spine. Everything else goes in an appendix — never bury the lead under twelve findings.

### Step 4 — Argue with the operator

Before writing, compare your ranking against the operator's raw read. Where they disagree, say so explicitly and defend your position — or concede and re-rank. This step is the difference between an audit and consultant slop. Surface the disagreement to the user; let them make the final call.

### Step 5 — Write the audit

Structure:

1. **Executive summary** — three sentences. The single biggest problem, what it costs, what fixing it is worth.
2. **Findings, ranked by ROI** — for each: the problem, the evidence, the cost, the fix, the effort. One page max per finding.
3. **Roadmap** — the fixes sequenced over 30/60/90 days. Quick wins first.
4. **Appendix** — lower-ROI findings, one line each.

Voice: direct, specific, no hedging, no consulting jargon. Every claim traceable to the source material or a stated assumption.

### Step 6 — Emit the goals file

Alongside the audit, write a `GOALS.md`: the roadmap converted into long-horizon goals with acceptance criteria, ordered by the ROI ranking. This is the file a cheaper model executes against in future sessions — each goal must be self-contained enough to work on without re-reading the audit.

## Outputs

- `audit-<client-name>.md` — the client-facing audit
- `GOALS.md` — the execution roadmap for follow-on agent sessions

---

*From the Turi Labs AI Agent Learn series — [turilabs.in](https://turilabs.in)*
