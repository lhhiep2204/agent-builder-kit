---
description: "Use when creating or updating Copilot customization files in .github, including agents, skills, instructions, prompts, templates, and hook guidance for any language and platform."
applyTo: ".github/**/*.md"
---

# Customization File Standards

Apply these rules when editing Copilot customization files in this repository.

## File Purpose

- Each file should serve one primary purpose
- Prefer composition across agent, skill, instruction, and prompt files instead of one overloaded file
- Do not create extra artifacts unless they improve execution quality or reuse

## Frontmatter

Follow all YAML frontmatter rules in SKILL.md. Additional rules for instruction file editing:
- Only use frontmatter keys listed in the "Supported Frontmatter Keys" table in `copilot-docs-registry.md`
- Keep frontmatter minimal and valid YAML
- Quote `description` values when punctuation could break parsing
- Make `description` concrete and searchable with trigger phrases the agent can match
- Use `applyTo` only for guidance that should load automatically
- Keep `applyTo` narrow; avoid broad patterns unless the rule truly applies everywhere in scope
- When authoring path-specific instructions, consider whether `excludeAgent` is needed to limit the instruction to coding-agent or code-review usage
- When authoring custom agents for VS Code workflows, keep surface-specific fields such as `agents`, `handoffs`, `user-invocable`, and `disable-model-invocation` aligned with current official documentation
- `agents` frontmatter MUST use inline JSON-style array with exact display names (`agents: ["Name A", "Name B"]`), NEVER block list syntax or filenames
- `description` frontmatter MUST be a double-quoted string

## Writing Standard

- Prefer explicit operating rules over broad aspirations
- State goals and non-goals when ambiguity is likely
- Define decision criteria when multiple primitives are possible
- Include validation checklists for complex or failure-prone workflows
- Keep examples realistic and domain-specific
- Make platform, framework, and technology assumptions explicit when the scope is technology-specific

## Primitive Selection

When creating a new customization file, start from the matching scaffold template in `.github/templates/agent-builder/`. Replace all placeholder text with project-specific content — never ship template boilerplate.

- Agent: `agent-template.md` — persona, context isolation, multi-stage workflow
- Skill: `skill-template.md` — reusable workflow with steps, templates, or assets
- Instruction: `instruction-template.md` — always-on guidance for matching contexts
- Prompt: `prompt-template.md` — short user-facing entry point
- Hook: `hook-checklist.md` — deterministic enforcement only when necessary and safe

For primitive selection decisions, apply `primitive-decision-matrix.md`. For role naming, reference `role-catalog.md`.

## Quality Checks

Before finalizing any non-trivial customization file, run the checklist in `.github/templates/agent-builder/agent-audit-rubric.md`. A file passes when it is specific, discoverable, and correctly scoped — not merely readable.

- Description is strong enough to trigger discovery
- Naming is consistent with the folder or file purpose
- Content does not duplicate another file without adding value
- Scope is as narrow as possible without breaking usefulness
- Output contract is clear where the file drives multi-step work
- Analyzer, generator, and auditor roles are split only when the separation adds quality
- Technology-specific files explicitly state platform, framework, concurrency, testing, or lifecycle expectations where needed

## Kit Entry Points

- **`/generate-workflow-agents`** — Generate, verify, or improve a full development workflow agent kit
- **`/add-agent`** — Add a new agent to an existing ecosystem, or verify and improve a single existing agent
- **`/refresh-agents`** — Re-analyze an evolved codebase and surgically update drifted agents, instructions, and skills

## Primary Work Surfaces

- `.github/agents/` for custom agents
- `.github/skills/` for reusable workflows and packaged assets
- `.github/instructions/` for path-specific rules
- `.github/prompts/` for user-facing entry points
- `.github/templates/agent-builder/` for scaffolds, decision assets, audit aids, and kit reference files

## Kit Maintenance Guidelines

When changing generation behavior or templates:
- when updating Copilot product behavior rules, use `Universal Copilot Docs Refresher` to refresh `.github/templates/agent-builder/copilot-docs-registry.md` from sources listed in its Mandatory Sources table
- when updating community skill knowledge, use `Universal Copilot Docs Refresher` to refresh `.github/templates/agent-builder/community-skill-registry.md` from upstream community skill repositories
- Technology and platform guidance should be codebase-first: strengthen the analyzer, generator, and auditor together so technology-specific rules come from target-project evidence rather than a separate snapshot
- `Universal Copilot Docs Refresher` is a kit-maintenance-only tool — it is NOT invoked during target project generation
- prefer current official GitHub documentation over stale repo examples if they conflict
- keep each customization file single-purpose
- keep descriptions concrete and discoverable; they are the discovery surface
- keep `applyTo` narrow and intentional
- do not add hooks, MCP frontmatter restrictions, or extra artifacts unless they materially improve execution quality
- generated agents should leverage MCP tools at runtime when configured — "avoid MCP in frontmatter" means do not restrict MCP access, not do not use MCP
- ensure generated agents align to the target project's actual technology stack, not kit fallback defaults
- derive build and test commands from the project's actual build system; never hardcode commands
- when a linter is configured, ensure verify-fix loops use strict mode to catch warnings, not just errors

Workflow-specific guidance belongs in skills, prompts, agents, and path-specific instructions — not in this file.

## Generation Quality Standards

Generated bundles must comply with SKILL.md artifact requirements, bundle shapes, and generation principles. The generator's SKILL.md Compliance Checklist in `universal-agent-generator.agent.md` is the authoritative verification list.

Key invariants this instruction enforces (SKILL.md is SSoT for each):
- 8 Generation Principles embedded in every generated agent
- Three-layer context model (SKILL.md > Context Optimization Rules)
- All structured clarification via `vscode_askQuestions` (SKILL.md > Single-Session Completion Rule)
- `handoffs:` in frontmatter for delegating agents (SKILL.md > Handoffs Pattern)
- `<!-- Generated by Agent Builder Kit -->` marker after frontmatter (SKILL.md > Generated File Marking)
- Evidence standard enforced (SKILL.md > Evidence Standard)
- Business knowledge in lightest effective primitive (SKILL.md > Business Knowledge Persistence)
- Five-agent baseline plus separated review pipeline by default; extra specialist roles only with analyzer evidence (SKILL.md > Bundle Shapes > Core Collaboration Coverage)
- Technology alignment to project's actual stack (SKILL.md > Universal Scope > Alignment rule)
- Build/test/lint commands derived from project (SKILL.md > Artifact Requirements > Implementor)

For the complete artifact/pattern list, see the Compliance Checklist in `universal-agent-generator.agent.md`.

Before finalizing changes to the kit:
- validate generated or edited customization files against `.github/templates/agent-builder/agent-audit-rubric.md`
- verify the generator flow and audit flow are consistent with each other
- call out residual risks explicitly instead of claiming certainty the repo has not earned

## Avoid

- vague descriptions
- bloated files that combine multiple roles
- duplicated process text across every artifact
- aggressive `applyTo` patterns that waste context
- hook recommendations without a safety rationale
- generic boilerplate in technology-specific bundles
