# Copilot Docs Registry

## Metadata

- Source repository: github/awesome-copilot
- Last refresh date: 2026-04-17
- Refresh method: MCP GitHub + web fetch
- Note: This is the kit's persistent snapshot of Copilot customization knowledge from `github/awesome-copilot`. Updated in place when the kit owner runs `Universal Copilot Docs Refresher`. The generator reads this file directly during generation — no per-run fetch is needed.

## How This File Is Used

1. **Kit maintenance**: The kit owner runs `Universal Copilot Docs Refresher` periodically. The refresher fetches the current state of `github/awesome-copilot` (Learning Hub, example files, `llms.txt`), then updates this file in place.
2. **Generation runtime**: The generator reads this file directly for Copilot product behavior rules (frontmatter allowlist, tool behavior, hook schema, instruction precedence). No per-run fetch is needed.
3. **Staleness tolerance**: Copilot behavior evolves with product releases. A snapshot up to 30 days old is acceptable. Beyond that, the kit owner should refresh before major generation runs.

---

## Sources Consulted

### Upstream Source
- `github/awesome-copilot` — Learning Hub articles, example agents/skills/instructions/hooks/plugins/workflows, cookbook entries. Confirmed current frontmatter conventions, agent/skill/instruction/hook architecture, MCP patterns, and surface-specific behavior.
- `llms.txt` is published on the website (`awesome-copilot.github.com/llms.txt`), not in the repository root.

### New Primitives Discovered (since 2026-03-21)
- **Plugins** — Curated bundles of agents and skills organized around themes, workflows, or use cases. Installable via `copilot plugin install <plugin-name>@awesome-copilot`. Browsable in VS Code (`@agentPlugins`, Command Palette → `Chat: Plugins`) and Copilot CLI (`/plugin marketplace browse awesome-copilot`). Awesome Copilot is a default plugin marketplace — no registration required. ~60+ plugins in directory.
- **Agentic Workflows** — AI-powered GitHub Actions automations written in markdown, compiled to `.lock.yml` via `gh aw compile`. Triggered by schedules, events, or slash commands. Requires `gh aw` CLI extension.
- **Copilot SDK cookbook** — Copy-paste-ready recipes for building agentic apps with the GitHub Copilot SDK (cookbook/copilot-sdk/).

## High-Signal Product Facts

### Supported Frontmatter Keys

This is the authoritative allowlist for frontmatter keys the generator may emit. Any key not listed here must not be used — it will cause editor diagnostics or be silently ignored. Updated during kit maintenance when official documentation changes.

| File type | Supported keys |
|-----------|---------------|
| Agent (`.agent.md`) | `name`, `description`, `agents`, `tools`, `model`, `target`, `user-invocable`, `disable-model-invocation`, `mcp-servers`, `handoffs`, `hooks` |
| Prompt (`.prompt.md`) | `description`, `agent` |
| Instruction (`.instructions.md`) | `description`, `applyTo`, `excludeAgent` |
| Skill (`SKILL.md`) | `name`, `description` |
| Plugin (directory-based) | Plugins are directories, not individual files with frontmatter. They bundle agents and skills under a shared README.md. No generator-emittable frontmatter — generation targets agents/skills within plugins, not plugin manifests themselves. |
| Agentic Workflow (`.md` → `.lock.yml`) | Frontmatter keys not yet confirmed. Workflow files compile via `gh aw compile`. **Do not generate workflow files until schema is verified.** |

### Hard Requirements
- Custom agent files remain Markdown with YAML frontmatter; supported keys are listed in the table above
- `infer` is retired in current documentation and should not be treated as the preferred control surface
- Path-specific instructions still require narrow `applyTo` globs and can use `excludeAgent` to target coding-agent versus code-review behavior
- Hook files for GitHub Copilot coding agent live under `.github/hooks/`, use `version: 1`, and only `deny` is currently processed from customer `preToolUse` output
- Hook scripts should be deterministic, auditable, and fast; default timeout remains 30 seconds unless overridden
- Upstream hook examples now include: dependency-license-checker, governance-audit, secrets-scanner, session-auto-commit, session-logger, tool-guardian — confirming the hook surface covers pre/post tool use, session lifecycle, and error handling
- Plugins are installable via `copilot plugin install`; the generator should not produce plugin manifests directly but may generate agents/skills that fit within a plugin structure
- Agentic Workflow files compile to `.lock.yml` and run as GitHub Actions; generator must not emit workflow files until the frontmatter schema is confirmed

### Best-Practice Guidance
- Best-practice docs continue to push toward well-scoped tasks with explicit acceptance criteria and concrete validation expectations
- Repository-wide instructions should reduce repeated exploration by documenting build, test, validation, and layout facts that agents need frequently
- Path-specific instructions remain the right place for repeated file-type or path-based conventions instead of duplicating those conventions in every agent
- VS Code custom agents should use least-privilege tool sets and explicit subagent or handoff design when sequential workflows matter
- Plugins provide a distribution and discovery mechanism for thematically grouped agents + skills; upstream examples show plugins organizing MCP development toolkits, framework migration suites, and language-specific best-practice bundles
- The ecosystem now includes language-specific MCP server development plugins (C#, Go, Java, Kotlin, PHP, Python, Ruby, Rust, Swift, TypeScript) — confirming that MCP server patterns are a first-class Copilot customization concern
- Upstream hook examples demonstrate governance-oriented patterns (license checking, secret scanning, tool access control) alongside productivity patterns (session logging, auto-commit) — reinforcing that hooks serve both safety and workflow automation

### Copilot Runtime Model

Understanding the runtime execution model is critical for generating effective agents and skills. The generator should encode this knowledge into generated bundle guidance so that generated agents, orchestrators, and user playbooks operate within the runtime's actual behavior.

#### What Enters the Model Context
The model does not see raw IDE state. Only what the prompt builder selects, renders, and retains after compaction reaches the model:

| Category | Examples | How it enters |
|---|---|---|
| User request | Prompt text, references, attachments, mode instructions | Rendered as user message |
| Project instructions | `copilot-instructions.md`, matched `.instructions.md`, settings | Rendered as system/user instruction blocks |
| Conversation history | Previous turns and summaries | Rendered as history or summarized history |
| Tool transcripts | Previous tool calls, arguments, and results | Rendered into later prompt rounds |
| Workspace context | OS, workspace structure, active document | Rendered as global agent context |
| Hook context | `sessionStart`, `userPromptSubmit` injections | Appended by hook system |
| Tool schemas | Available tools for the current round | Sent alongside messages |

#### Tool-Calling Gate
Not every chat turn is tool-calling by default. Tools must be exposed by the runtime before the model can use them:
- Agent paths can expose tools when runtime, model capability, and current mode allow it
- A plain panel chat path may stay non-tool-calling if the intent does not override tool exposure
- No exposed tools = no tool calls, even if prompt wording asks for them
- When a workflow depends on tools, generated agents should document what tools they expect

#### Tool Result Round-Trip Timing
Tool results from round N appear in the prompt for round N+1, not in the same round. This is a fundamental constraint:
- The model emits `tool_calls` in round N
- The runtime executes the tool between rounds
- The model sees the result in round N+1 and continues reasoning
- Generated agents and skills that depend on multi-step tool workflows should account for this sequencing

#### Context Transformation Pipeline
Between local state and the final model request, data passes through 5 stages:
1. **Request normalization** — sanitize references, resolve location, rebuild conversation from history
2. **Prompt context assembly** — gather query, history, tool results, chat variables, available tools, mode instructions, hook context
3. **Intent-specific prompt rendering** — the prompt builder selects which blocks to include
4. **Budget enforcement** — truncation, summarization, or re-rendering if context exceeds limits
5. **Message post-processing** — final messages and tool schemas formatted for the model endpoint

Each stage can drop, summarize, transform, or add context.

#### Compaction and Summarization
Long-running threads do not break the system — the runtime manages context budget:
- Token budget measured before each prompt render
- Near-limit: background or foreground summarization compresses conversation history
- Summary metadata persisted and used for re-rendering
- Global context may be cached across turns if cache key is still valid
- Implication for generated bundles: keep one thread per feature/subsystem; trust compaction to handle length

#### Continuation Loop
The execution loop can continue beyond a single round for multiple reasons:
- The model emitted tool calls for the next round
- A stop or subagent-stop hook blocked stopping and turned its reason into a continuation query
- Autopilot or task-oriented execution decided the task is not complete yet
- The loop stops only when the runtime decides it is allowed to stop and the task is considered complete

#### `.github/` File Classification for Prompt Assembly

| Group | Files | Prompt behavior |
|---|---|---|
| **A — GitHub Infrastructure** | `.github/workflows/*`, `dependabot.yml`, `CODEOWNERS`, `ISSUE_TEMPLATE/*` | NOT auto-injected into Copilot prompts |
| **B — Copilot Prompt Resources** | `copilot-instructions.md`, `instructions/*.instructions.md`, `agents/*.agent.md`, `prompts/*.prompt.md`, `skills/*/SKILL.md` | Resolved by platform customization pipeline; injected when scope matches |
| **C — Governance & Supporting Docs** | `constitution.md`, docs, templates, dependency maps | Pull resources: loaded only when explicitly requested by a skill/agent |

Generated bundles should educate users about this classification — Group B files are the only ones automatically assembled into prompts.

### Official Hook Events

These are the confirmed GitHub Copilot hook events for `.github/hooks/*.json`:

| Event | When it fires | Typical use |
|---|---|---|
| `sessionStart` | Session begins | initialize state, session logging |
| `userPromptSubmitted` | User submits a prompt | audit, logging, prompt gating |
| `preToolUse` | Before a tool executes | security gates, confirmation, input checks |
| `postToolUse` | After a tool executes | formatter, lint, compile, usage logging |
| `preCompact` | Before context compaction | export decisions, constraints, plan state |
| `subagentStart` | A subagent is spawned | track nested agent lifecycle |
| `subagentStop` | A subagent completes | aggregate results, audit trail |
| `stop` | Session ends | generate reports, send notifications, cleanup |

**Critical**: Only `deny` is currently processed from customer `preToolUse` output. Use `postToolUse` with tool filtering for expensive quality checks. `preCompact` hooks must be fast (<10s) — they block compaction. The generator and hook-checklist must reference this table as the authoritative event list.

### Maintainer Debug Recipe

When runtime behavior looks wrong, inspect in this order:
1. **Prompt profiler** — inspect prompt shape, included context, and whether the right instructions or tool schemas were present
2. **Agent debug log** — inspect orchestration, routing, and continuation behavior across turns
3. **Chat Debug View** — inspect the concrete prompt, tool calls, and tool results for a real session
4. **OTEL content capture** — enable only when deeper tracing needed

Generated user playbooks should include a troubleshooting section that references this inspection order.

### Surface-Specific Caveats
- GitHub.com and IDEs still diverge on some properties; VS Code supports `agents`, `handoffs`, and scoped hooks, while GitHub.com ignores some IDE-only properties
- Tool behavior and availability remain surface-dependent; generator and auditor rules should continue to call out those differences rather than assuming uniform behavior
- Plugin install (`copilot plugin install`) works in both Copilot CLI and VS Code; VS Code also supports `@agentPlugins` search and `Chat: Plugins` command palette
- Agentic Workflows require the `gh aw` CLI extension and compile to GitHub Actions `.lock.yml` — they are a CI/CD surface, not an IDE surface

### Deprecated Or Preview Concerns
- IDE custom-agent behavior outside VS Code remains subject to preview caveats in current GitHub docs
- VS Code scoped hooks remain preview-gated and should not be treated as universally available
- Agentic Workflows (`gh aw compile`) are a new primitive with no confirmed stable frontmatter schema; treat as preview until schema is documented in official product docs
- Plugin marketplace integration appears stable (default marketplace, no registration required) but the plugin manifest format may still evolve

## Implications For Generation

- The kit maintains this file as a persistent reference; the generator reads it directly during generation without per-run fetching. The kit owner should re-run `Universal Copilot Docs Refresher` periodically or before major rule updates.
- Generated workflow kits should prefer repo-grounded instructions, prompts, validation guidance, and templates over generic prose because current docs reinforce scoped tasks and concrete validation
- For portability across environments, generated agents should avoid frontmatter `tools` and `mcp-servers` by default and encode behavior constraints in instructions unless the user explicitly asks for constrained tool access
- Hook generation must stay conditional and justified; a no-hook rationale is better than speculative hook output when deterministic commands are not available
- The auditor and quick rubric should both check for concrete validation guidance and orchestrator guardrails so lightweight audits do not miss hard requirements
- The generator should not emit plugin manifests or agentic workflow files until their schemas are confirmed; generated agents and skills may later be grouped into a plugin structure as a follow-up
- Upstream hook diversity (governance + productivity patterns) validates the kit's hook generation approach: only emit hooks when deterministic, auditable commands exist for the use case

## Conflicts With Existing Kit Rules

- The previous kit state allowed a full workflow bundle to pass while generating mostly agents; that no longer matched the documentation emphasis on explicit guidance, repeatable instructions, and validation support
- The previous kit state could overreact to unrelated dirty-worktree changes instead of preserving user edits and continuing safely
- The kit's `llms.txt` fetch step previously assumed the file was in the repo root; it is on the website (`awesome-copilot.github.com/llms.txt`) — refresh instructions below have been updated to reflect this

## Recommended Follow-Up Changes

- Require supporting skill, instruction, prompt, and template/checklist coverage by default for full workflow generation, unless analysis proves strong existing artifacts already cover those needs
- Require repo-grounded validation commands, micro-change handling, skip rules, and reviewer conflict resolution in generated workflow bundles
- Preserve unrelated worktree changes and escalate only for direct path conflicts with target generated files
- Investigate agentic workflow frontmatter schema (from `gh aw` CLI docs or upstream workflow examples) and add a confirmed row to the frontmatter allowlist table when stable
- Consider generating plugin-ready directory structures when producing multi-agent bundles, so users can optionally package and distribute them
- Evaluate upstream hook examples (governance-audit, secrets-scanner, tool-guardian) as templates for hook generation patterns

## Residual Risks

- Current guidance remains surface-sensitive, especially for IDE preview features and scoped hooks
- Generated instruction quality still depends on analyzer quality; if analysis misses stable scopes or validation commands, downstream artifacts can still be weaker than intended
- Agentic workflow frontmatter schema is unconfirmed — the generator must not emit workflow files until this is resolved
- Plugin manifest format stability is assumed but not guaranteed; early adoption of plugin-structured output could require rework
- The `llms.txt` machine-readable index was not fetched from the website during this refresh; if it contains additional keys or constraints not visible in the repo, those are missing from this snapshot

---

## Mandatory Sources

These pages must be consulted on **every** refresh.

| # | URL | Why It Matters |
|---|-----|----------------|
| 1 | https://code.visualstudio.com/docs/copilot/customization/custom-agents | VS Code custom-agent spec: frontmatter fields, tool control, subagent wiring, hooks, discovery rules |
| 2 | https://docs.github.com/en/copilot/reference/customization-cheat-sheet | Single-page overview of all primitives: instructions, skills, agents, prompts, hooks |
| 3 | https://docs.github.com/en/copilot/tutorials/coding-agent/get-the-best-results | Best-practice guide for coding-agent tasks: scoping, validation, acceptance criteria |
| 4 | https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-custom-agents | Conceptual model for custom agents on GitHub.com |
| 5 | https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents | Creation flow, filename constraints, cross-IDE notes |
| 6 | https://docs.github.com/en/copilot/reference/custom-agents-configuration | Frontmatter key reference, surface differences, tool aliases |
| 7 | https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/use-hooks | Hook placement, version expectations, debugging |
| 8 | https://docs.github.com/en/copilot/reference/hooks-configuration | Hook event types, input/output, timeout defaults |
| 9 | https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot | Repo-wide and path-specific instructions, excludeAgent, precedence |
| 10 | https://docs.github.com/en/copilot/concepts/agents/about-agent-skills | Skill conceptual model: folder structure, description-based loading |
| 11 | https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-skills | Skill creation flow and guidance |

## Optional Sources

Add rows here when a refresh reveals new pages worth tracking.

| # | URL | Why It Matters |
|---|-----|----------------|
| — | — | — |

---

## Refresh Instructions

When updating this file during kit maintenance:

1. Fetch the current `llms.txt` index from the website (`awesome-copilot.github.com/llms.txt`) via web fetch (it is NOT in the repo root)
2. Read Learning Hub articles from `docs/` folder: Building Custom Agents, Creating Effective Skills, Defining Custom Instructions, Automating with Hooks, Agents and Subagents, Copilot Configuration Basics, Understanding Copilot Context
3. Sample example agent/instruction/skill files for current frontmatter conventions
4. Walk linked Learning Hub articles, example agents, skills, instructions, hooks, plugins, workflows, and cookbook entries
5. Extract high-signal product facts: frontmatter keys, hard requirements, best-practice guidance, surface-specific caveats, deprecated patterns
6. Compare extracted facts against the current snapshot sections and update in place
7. Update the "Last refresh date" in the metadata section
8. If any upstream content is inaccessible, note it under a `### Unavailable Content` sub-section and keep previous facts

### Expected Sections

Each refresh should produce these sections with current upstream data:

- **Sources Consulted** — upstream source with summary of what was confirmed
- **High-Signal Product Facts** — frontmatter allowlist, hard requirements, best-practice guidance, surface-specific caveats, deprecated/sunset concerns
- **Implications for Generation** — how upstream facts constrain or inform the generator
- **Conflicts with Existing Kit Rules** — any contradictions between upstream and current kit behavior
- **Recommended Follow-Up Changes** — actionable improvements based on new upstream data
- **Residual Risks** — known gaps or uncertainties after the refresh

### Registry Maintenance Rules

1. **Add a source** when a refresh finds a new page that contains hard requirements or best-practice guidance affecting generation.
2. **Remove a source** when GitHub retires the page and the content is fully superseded.
3. **Never remove a mandatory source** without checking that its content has migrated elsewhere and updating the corresponding row.
4. The refresher agent must report any mandatory source that returns an error so the kit owner can investigate.
