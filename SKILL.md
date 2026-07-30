---
name: deep-research
description: Use when the user asks to "调研/研究/分析" a non-trivial topic and wants a rigorous, well-structured deliverable — especially when they mention 调研、研究、深度分析、对比分析、策略、方法论, or want a report with critical review. Runs a structured pipeline: align scope → fan out parallel research subagents → synthesize → critical review → output MD + designed HTML. This is the skill for any research task that deserves more than a single-pass answer.
---

# Deep Research

A structured research pipeline that produces rigorous, well-designed deliverables. The core idea: **don't answer a hard research question in one pass — decompose it, investigate in parallel, synthesize, then stress-test the synthesis with an independent critical review.**

## When to use

Trigger when the user wants a genuine research deliverable — signaled by words like 调研、研究、深度分析、对比分析、策略规划、方法论、复盘, or by the complexity of the question (multi-factor, needs real investigation, deserves a report rather than a chat reply).

Do NOT use for: quick factual lookups, single-file code questions, anything answerable in a few sentences. If unsure, briefly ask the user whether they want the full pipeline.

## The pipeline (5 stages)

Work through these stages in order. Each stage has a quality bar.

### Stage 1 — Align scope (before doing any work)

Do NOT jump into research. First align with the user on what the research is actually for. Ask a tight set of questions (use AskUserQuestion) covering:

- **Subject & boundary**: what exactly is being researched? what's explicitly out of scope?
- **Purpose & audience**: what decision will this inform? who reads it? (a board, yourself, a client)
- **Depth & output**: how deep? default is MD + HTML, but confirm — maybe they only want MD, or want extra formats (slides, sheet).
- **Known constraints**: time pressure, must-include angles, things they already believe (so you can challenge them).

Keep this short — 2-4 questions max, batched in one AskUserQuestion call. If the user's request is already fully specified, skip to Stage 2 and note the assumptions you're running with.

State the research plan back to the user in 3-5 lines (dimensions to investigate, parallel subagents planned, output format) so they can course-correct before you spend tokens.

### Stage 2 — Fan out parallel research

Decompose the topic into **independent investigative dimensions** and dispatch one subagent per dimension, in parallel (multiple Agent tool calls in a single message). Each subagent must be self-contained — it does NOT see the others' work.

**How many subagents? Scale to complexity, not a fixed number:**
- Simple / single-factor topic → 2 subagents
- Standard multi-factor topic → 3 subagents (the common case)
- Large / systemic topic → 4-5 subagents

**How to split dimensions** — aim for dimensions that are MECE-ish (mutually exclusive, collectively exhaustive) and each independently researchable. Bad split: "background / details / conclusion" (these overlap and telescope). Good split: "first-principles + metrics / positioning strategy / growth-stage playbook" (parallel, non-overlapping lenses).

**Each subagent prompt must include:**
1. The research subject and the user's specific situation/context (so findings are tailored, not generic).
2. That subagent's specific dimension and what it should answer.
3. A demand for density: concrete numbers, real cases, named examples — not platitudes. "不要写客套话，直接给硬货。"
4. Permission to use WebSearch/WebFetch for real, current info where relevant.
5. Output language (match the user's).

Tell each subagent explicitly that it is one of several parallel investigators working on independent dimensions — this prevents overlap and encourages depth over breadth.

After all subagents return, **read all of them before synthesizing** — don't synthesize from the first one that finishes.

### Stage 3 — Synthesize into MD

Write a single coherent MD report that integrates all subagent findings. This is NOT a stitched-together concatenation — reorganize into a logical structure that serves the user's purpose.

**Synthesis principles:**
- Open with a TL;DR / 核心结论速览 — the user should get the answer in the first screen.
- Resolve contradictions between subagents explicitly (don't paper over them).
- Re-order findings by importance to THIS user, not by which subagent produced them.
- Density over length. Cut filler, repetition, and throat-clearing.

**Mandatory: data honesty.** When the research contains benchmarks, thresholds, or "industry standards," add a visible data-honesty note stating: which numbers are official vs. industry-estimate vs. untraceable, and that platform algorithms/thresholds are black boxes where applicable. Never present an experience-based number as if it were a verified fact. This is non-negotiable — it's what separates a trustworthy report from a confident-sounding hallucination.

Save the MD to the user's working directory (ask for the path in Stage 1, or default to the current project dir).

### Stage 4 — Critical review (ask before escalating)

After synthesis, stress-test the report. There are two levels — **ask the user which they want** (default to self-check, offer escalation):

**Level 1 — Self-check (default):** Review the synthesized MD yourself for: logical gaps, unsupported claims, survivorship bias, numbers presented as fact that are actually estimates, recommendations that don't follow from the evidence. Fix or annotate what you find. Use this for most research, especially when the deliverable is descriptive/factual (market research, comparative analysis).

**Level 2 — Independent adversarial review (escalation):** Dispatch a separate subagent whose ONLY job is to attack the report. This subagent must:
- Be given the report file path, NOT the conversation context — so it judges the document on its merits, untainted by knowing how the conclusions were reached.
- Be explicitly told it was NOT involved in writing, should assume nothing, and its value is in finding flaws — no polite balancing.
- Attack across dimensions: logical rigor, data credibility (verify key numbers via WebSearch), survivorship bias, applicability to the user's actual situation, missing blind spots, unrealistic recommendations.
- Output specific, located fixes ("§X says Y, problem is Z, change to W"), not vague "could be better."

**When to proactively recommend Level 2** (don't wait to be asked): the research drives a consequential or hard-to-reverse decision, makes original strategic claims, will be published externally, or involves compliance/legal/financial risk. Offer it; let the user decide.

After review (either level), apply the fixes to the MD. Keep a short "修订日志" (revision log) appendix documenting what changed and why — this makes the review's value visible and auditable.

### Stage 5 — Output MD + designed HTML

Produce the final deliverables.

**MD:** the reviewed report from Stage 4.

**HTML (default):** a single self-contained, well-designed HTML rendering of the report. Load the `frontend-design` skill for design guidance. Design choices should serve the subject — see `references/design-reference.md` for a default design language and signature-element patterns to start from (do NOT copy it verbatim; adapt to the topic). Verify the HTML actually renders (open in browser, check key elements via DOM, fix issues) before declaring done.

**Confirm output format with the user in Stage 1** — if they only want MD, or want slides/sheet instead of HTML, respect that. The MD+HTML default is a starting point, not a requirement.

## Quality principles (apply throughout)

1. **Investigation before opinion.** Fan-out subagents exist to gather real evidence. Don't let the synthesis assert things the subagents didn't support.
2. **Show your work on data.** Every number gets a source-status tag. Experience estimates are fine — lying about them being official is not.
3. **The review must bite.** A review that only praises is a failed review. If Level 2 comes back with no substantive criticism, the review prompt wasn't adversarial enough — re-dispatch with sharper instructions.
4. **Density over volume.** A 15-page padded report is worse than a 5-page dense one. Cut relentlessly.
5. **Match the user's actual situation.** Generic best-practice advice is worthless. Every recommendation should be traceable to the user's specific context gathered in Stage 1.

## Reference files

- `references/design-reference.md` — default HTML design language (color, type, signature elements) to start from. Read before Stage 5.
- `references/adversarial-review-prompt.md` — a starter prompt for the Level 2 independent adversarial review subagent. Read when escalating to Level 2.

## One-line summary

Decompose → parallel investigation → synthesize with data honesty → stress-test (self-check by default, escalate to independent adversarial review for high-stakes) → output MD + designed HTML.
