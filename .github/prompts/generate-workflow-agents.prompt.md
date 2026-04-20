---
description: "Generate, verify, or improve a full development workflow agent kit for any language and platform: analyze the repo, then create, audit, or update agents with quality gating."
agent: "Agent Builder"
---

# Generate Workflow Agents

Analyze the codebase first, classify project state, then act based on what exists:
- **Greenfield** (no agents): generate all agents from scratch
- **Established** (agents exist): run drift detection first (stale file refs, outdated conventions, abandoned patterns), then evaluate and improve existing agents, fill gaps, fix weak collaboration
- **Mixed** (partial setup): preserve working agents, fill gaps

Do not start with a broad interview — extract context by analyzing the codebase directly, then ask only if critical architecture-changing facts remain missing.

## Standard User-Facing Progress

1. Analyze the codebase
2. Assess the project and choose bundle architecture
3. Ask targeted follow-up only if needed
4. Generate or update the bundle
5. Audit and iterate until PASS
6. Deliver the final result

Do not add extra top-level phases for internal setup. Phase 3 auto-skips when no ambiguity remains.

Complete the entire workflow through delivery in this session. When user input is needed, use `vscode_askQuestions` to ask with structured options and wait for the response, then continue. Never write questions as plain text. Never leave generation incomplete.

## Role Coverage

Use SKILL.md as the single source of truth for role behavior and collaboration contracts.

- Baseline delivery: Business Analyst, Investigator, Implementor, Test Specialist, Dev Orchestrator + separated review pipeline
- Scale-up roles for very large/high-risk programs: add only when analyzer evidence warrants (for example DevOps, Security, Performance, API, Database, Documentation, Architect, Migration, Release Readiness)

## Execution Steps

1. **Analyze** — Deeply scan project: architecture, conventions, domain, existing agents, build setup, testing, delivery workflow. Classify project state. Recommend where shared business knowledge should live: project context instruction only, domain-scoped instructions, a business domain registry / domain map, or a reusable business-domain skill.
2. **Assess** — Summarize: project state, constraints, recommended bundle shape, gaps, workflow-family coverage matrix.
3. **Clarify only if needed** — Ask minimum targeted questions only if a missing fact would materially change architecture.
4. **Generate** — Produce full workflow kit per SKILL.md bundle shape. For established projects: improve weak agents, add missing roles, tighten collaboration contracts. Reference real project conventions. Name generated files with project-derived prefix, while treating `.github/copilot-instructions.md` as the standard non-prefixed workspace-level file: create it if missing, or update it if present while preserving relevant existing content. Also generate `<prefix>-project-context.instructions.md` for deeper reference context. When project business complexity warrants it, generate the appropriate shared business knowledge artifact and wire it into the relevant agents' inputs and decision rules.
5. **Audit** — Full audit including ecosystem coherence. Every finding (major and minor) must be resolved.
6. **Revise** — If `REVISE`, fix every finding and re-audit. Do not accept or skip minor findings. Repeat until `PASS` with zero open issues.
7. **Deliver** — Complete bundle with architecture rationale and residual risks (only genuine uncertainties, not fixable weaknesses).

Final quality expectation: zero open findings (including minor) and zero editor diagnostics in generated agent files before completion.

## Constraints

Follow all SKILL.md requirements, bundle shapes, and the generator's SKILL.md Compliance Checklist. Flow-specific constraints:

- Every generated role must reference real project conventions, not generic boilerplate
- Project-derived file prefix for generated artifacts
- `.github/copilot-instructions.md`: create if missing, update if present (preserve relevant existing content)
- Shared business knowledge in the lightest effective primitive; no orphaned business artifacts
- Bidirectional cross-references between agents and skills; delete intermediate files before finalizing
- For established projects: preserve working behavior while fixing weak areas
- Skip full 95% confidence interview — rely on analysis first
- Final quality: zero open findings (including minor) and zero editor diagnostics before completion
