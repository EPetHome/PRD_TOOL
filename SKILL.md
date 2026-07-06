---
name: ai-prd-suite
description: "Run the user's AI 产品 PRD 三模块套件 workflow. Use when the user asks to create, convert, validate, or connect the PM suite deliverables: module 2 human-readable PRD Markdown, module 3 AI 构建包, module 3 executable-readiness review, module 1 clickable HTML prototype, module 1+2 three-column review page, or full-chain consistency checks for files generated from PM套件-V1.0."
---

# AI PRD Suite

## Purpose

Operate the user's "AI 产品 PRD 三模块拆离" workflow as an execution layer over the PM suite. Treat the PM suite Markdown files as the standard source, and help the user generate or validate the four practical deliverables:

- Module 2: human-readable PRD Markdown.
- Module 3: AI-readable build package Markdown.
- Module 1: clickable single-file HTML prototype.
- Module 1+2: three-column comprehensive review HTML.

## Source Of Truth

Prefer the live suite if the current workspace contains it:

`PM套件-V1.0/`

If the live suite is unavailable, use this skill's bundled snapshots in `references/`:

- `references/readme-quickstart.md`
- `references/module-2-doc-template.md`
- `references/module-3-output-spec.md`
- `references/module-3-conversion-prompt.md`
- `references/module-1-prototype-scaffold.md`
- `references/module-1-2-review-page-scaffold.md`

Load only the reference files needed for the requested task. Do not bulk-load all references unless doing a full-chain audit.

## Operating Rules

- Work on the user's project files, not on PM suite template files, unless the user explicitly asks to edit the suite.
- Preserve the separation of concerns:
  - Module 1 = interaction prototype.
  - Module 2 = human-readable narrative PRD.
  - Module 3 = AI-executable structured build package.
  - Module 1+2 = derived review page, not a new source of truth.
- Do not treat sample/test outputs as templates to edit unless the user explicitly asks. Use them to diagnose template gaps.
- When generating files, save them beside the source project files unless the user gives another location.
- Use the naming conventions from the suite:
  - `[[ 产品名 ]]-模块 2-文档.md`
  - `[[ 产品名 ]]-AI构建包.md` or `[[ 产品名 ]]-AI 构建包.md`
  - `[[ 产品名 ]]-原型.html`
  - `[[ 产品名 ]]-综合评审页.html`
- For HTML artifacts, produce a single file that can be opened directly in a browser.

## Task Router

### If The User Asks To Fill Or Review Module 2

Use `module-2-doc-template.md`.

Check or produce:

- Chapters 1-8 are present.
- Chapter 8 "下游转译输入" is filled because Module 1 and Module 3 depend on it.
- Module 2 does not contain database schema, API paths, JSON schema, confidence thresholds, model parameters, detailed UI states, or button-level UI specs.
- Unclear decisions are marked `[待产品方补充]` rather than invented.

When reviewing, lead with missing blockers and boundary violations. When generating, keep it narrative and human-readable.

### If The User Asks To Generate Module 3

Use `module-3-conversion-prompt.md` and `module-3-output-spec.md`.

Input: reviewed Module 2 Markdown.

Produce a Markdown AI build package with:

- Chapters 0-8 in order.
- Technical choices from Module 2 chapter 8.5 first, then chapter 5 if needed.
- Data models derived from Module 2 chapters 8.2, 8.3, and 8.4.
- Page structure, interaction flows, AI decision points, rule points, compliance guardrails, and execution order.
- A final self-check section containing naming unification, field uniqueness, fuzzy-word scan, compliance insertion list, pending-product items, and source mapping.

Do not invent product decisions. Use `[待产品方补充]` for values that require PM judgment.

After generating Module 3, immediately run the executable-readiness review below.

### If The User Asks To Validate Whether Module 3 Is AI-Executable

Use the "可执行性验证 Prompt" logic from `module-3-conversion-prompt.md`.

Input: Module 3 AI build package only. Do not consult Module 2 unless the user asks for a repair plan.

Return:

- Whether it can be handed directly to Cursor / Claude Code / v0.
- Blocking issues.
- Non-blocking risks.
- Executability matrix.
- If pass: one concise construction instruction for the build AI.
- If fail: say not to start and list what to fix first.

Treat these as blockers:

- Missing or vague technical choices.
- `[待产品方补充]` in a field required to start building.
- Missing data model, page structure, interaction flow, AI/rule logic, compliance insertion point, or execution order.
- Entity / field / page / flow names that do not align.

### If The User Asks To Generate Module 1 Prototype

Use `module-1-prototype-scaffold.md`.

Input: reviewed Module 2 Markdown.

Produce a single-file HTML prototype with:

- Left floating control console card, 280-320px wide.
- Right iPhone model with 390x844 screen content.
- Tailwind CDN plus native JavaScript only.
- No iframe and no postMessage.
- Console controls only major demo variables, not main navigation.
- Phone-internal click flow using route state, e.g. `navigate`, `goBack`, `showToast`, `openModal`.
- Every clickable-looking button, card, tab, list item, or back arrow has a result.
- P0 journeys and core functions from Module 2 have pages, modals, state changes, or explicit placeholder toast.
- Normal / failure / empty / loading states are reachable.

If the user asks for validation, check the HTML source for dead-button risk, iframe/postMessage, 390x844 phone shape, and whether the phone can progress without using the console.

### If The User Asks To Generate The Comprehensive Review Page

Use `module-1-2-review-page-scaffold.md`.

Inputs:

- Reviewed Module 2 Markdown.
- Accepted Module 1 prototype HTML.

Produce one single-file HTML review page with exactly three desktop columns:

1. Left: switch console only.
2. Middle: interactive iPhone prototype only.
3. Right: readable PRD document with TOC.

Do not output a two-column page where "console + phone" are both on the left. If the Module 1 HTML has console and phone together, split the console DOM into the left column and the phone DOM into the middle column while preserving state and render functions.

Do not use Module 3 content in the review page.

### If The User Asks For Full-Chain Validation

Inspect the available artifacts:

- Module 2 Markdown.
- Module 3 AI build package.
- Module 1 prototype HTML.
- Module 1+2 review page HTML.

Check:

- Module 2 chapter 8 feeds both Module 1 and Module 3.
- Module 3 fields, pages, flows, rules, and compliance items map back to Module 2.
- Module 1 prototype covers Module 2 journey and core functions without becoming a PRD document.
- Review page is three-column: left switcher, middle phone, right PRD.
- The artifacts do not contradict each other on product scope, roles, states, compliance, or technical choices.

Report findings as blockers first, then non-blocking risks, then recommended next action.

## Repair Strategy

When something fails:

- If the missing item is a product decision, ask to fix Module 2 or mark `[待产品方补充]`.
- If the issue is structure or naming in Module 3, repair Module 3 without changing Module 2.
- If the issue is prototype interaction or layout, repair Module 1 or the Module 1 scaffold depending on whether the problem is one-off or systemic.
- If the issue appears in multiple generated projects, recommend updating the PM suite template, not just the sample artifact.

## Output Style

Be practical and file-oriented. For generation tasks, create or update the artifact when enough input files are available. For review tasks, keep findings concise and actionable, with file paths and line references when useful.
