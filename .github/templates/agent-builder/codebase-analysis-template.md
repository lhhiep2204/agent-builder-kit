# Codebase Analysis Template

## Project State

- classification: <greenfield | established | mixed>
- evidence:

## Existing Customization Inventory

- repository-wide instructions:
- path-specific instructions:
- prompts:
- skills:
- agents:
- hooks:

## Naming And Discovery Conventions

- file naming pattern:
- agent naming pattern:
- description style:
- trigger phrase quality notes:

## Role Comparison Matrix

| Artifact | Role | Primary responsibilities | Non-goals / boundaries | Quality notes | Overlap risks |
|----------|------|--------------------------|-------------------------|---------------|---------------|
| <file> | <role> | <responsibilities> | <non-goals> | <quality note> | <overlap note> |

## Project Surface

- platforms:
- project type:
- build system:
- targets and modules:
- package dependencies:
- CI or release workflow:

## Validation And Automation Surface

- local build commands:
- local test commands:
- default test configuration: <derived from project config — never hardcode specific device/environment values; use the project config to determine the correct defaults>
- multi-target or multi-module checks:
- environment constraints:
- deterministic commands safe enough for hook consideration:
- linting tools in use:
- lint tool handling recommendation (instruction vs hook):

## Harness Engineering Assessment

### Feedforward Controls (Guides)
- existing architecture documentation:
- existing coding conventions in repo:
- existing agent guidance files (AGENTS.md, copilot-instructions.md):
- existing structured templates for tasks/features/PRs:
- type system strictness (implicit feedforward):

### Feedback Controls (Sensors)
**Computational sensors** (deterministic, fast):
- linters: <tool, config location, active rules, custom error messages?>
- type checking: <strictness level>
- test suites: <speed, reliability, can run in verify-fix loops?>
- structural analysis: <tools>
- CI checks: <what runs automatically>

**Inferential sensors** (semantic, non-deterministic):
- existing code review agents/tools:
- AI review or quality scoring tools:

### Harnessability Rating
- rating: <high | medium | low>
- rationale: <why — strong typing, clear boundaries, existing tooling = high; weak typing, tangled deps = low>
- ambient affordances: <framework conventions, file organization clarity, generated code patterns>
- harness gaps: <what feedforward/feedback controls could generated agents fill?>

## Delivery Workflow Behavioral Signals

- verify-fix commands (build, test, lint) with expected timeouts:
- review methodology depth (diff-only vs context-deep):
- functional/technical review separation:
- investigation methodology (as-is/to-be, impact matrix, sequence diagrams):
- hand-off pattern between investigation and implementation (confirmation checkpoint, structured report, informal):
- team size signal (solo, small agile, larger delivery):

## Recommended Collaboration Topology

- primary workflow lanes:
- default delivery sequence:
- default iteration loops by lane:
- auto-iteration boundaries:
- escalation boundaries back to the user:

## Specialist Hand-Off Contract Candidates

| Upstream role | Downstream role | Required input | Expected output | Pass criteria | Revise criteria | Blocked criteria |
|---------------|-----------------|----------------|-----------------|---------------|-----------------|------------------|
| <role> | <role> | <input> | <output> | <pass> | <revise> | <blocked> |

## Workflow Family Coverage Matrix

| Workflow family | Priority (primary / recurring / optional / omit) | Likely roles | Normal hand-off order | Iteration loop | Escalation boundary | Supporting artifacts |
|-----------------|---------------------------------------------------|--------------|-----------------------|----------------|--------------------|----------------------|
| Planning / story shaping | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Investigation / diagnosis | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Feature delivery | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Testing / validation | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Review | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Ecosystem extension | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |
| Verify / improve existing ecosystem | <priority> | <roles> | <order> | <loop> | <escalation> | <artifacts> |

## Candidate Instruction Scopes

- implementation paths and patterns:
- test paths and patterns:
- UI-specific paths and patterns:
- data layer paths and patterns:
- review or documentation paths and patterns:
- narrow `applyTo` recommendations:
- distinct convention domains that warrant separate instructions:

## Candidate Skill Domains (Evidence-Backed Only)

- delivery workflow skill:
- testing methodology skill (serves which agents):
- investigation methodology skill (serves which agents):
- review methodology skill (serves which agents):
- other domain skill candidates with evidence + multi-agent reuse:

## Candidate Prompt Entry Points

- primary entry point (e.g., deliver feature):
- secondary entry points (e.g., investigate bug, generate tests, review code):
- planning or story-shaping entry points:
- ecosystem verification or upgrade entry points:

## Candidate Template And Checklist Assets

- feature delivery packet / handoff checklist:
- investigation report template:
- business domain registry / domain map:
- planning or story-shaping packet:
- validation plan or test packet:
- review packet:
- ecosystem extension or ecosystem improvement checklist:
- other recurring hand-off templates:

## Architecture

- layer model:
- feature structure:
- navigation pattern:
- state management pattern:
- dependency injection pattern:
- data and persistence pattern:

## Technology Stack And Alignment Profile

- primary language(s) and version(s):
- frameworks in use:
- build system:
- testing frameworks:
- concurrency model:
- linting/formatting tools:

### Technology Alignment

| Dimension | Project actual | Detected version/tool | Notes |
|-----------|---------------|----------------------|-------|
| Language version | <detected> | <version> | <migration state if any> |
| Primary framework | <detected> | <version> | <legacy fallback if any> |
| Build system | <detected> | <tool> | |
| Testing framework | <detected> | <tool> | |
| Concurrency model | <detected> | <approach> | |
| Persistence | <detected> | <tool/pattern> | |
| Linter/formatter | <detected> | <tool + config> | |
| Migration state | <stable or migrating X→Y> | n/a | <note> |

Generated agents must align to the "Project actual" column. Note deviations explicitly so the generator does not apply generic defaults where the project uses different technology.

## Technology Domain Coverage Matrix

| Domain | Signal strength | Evidence files / config | What generated agents must know | Preferred placement |
|--------|-----------------|-------------------------|---------------------------------|---------------------|
| UI composition and rendering | <strong / moderate / thin / unknown> | <files, config, or commands> | <critical guidance> | <project context instruction / path-scoped instruction / agent instruction / skill / skip> |
| State management | <signal> | <evidence> | <guidance> | <placement> |
| Data persistence and flow | <signal> | <evidence> | <guidance> | <placement> |
| Concurrency and async | <signal> | <evidence> | <guidance> | <placement> |
| Testing surface | <signal> | <evidence> | <guidance> | <placement> |
| API and networking | <signal> | <evidence> | <guidance> | <placement> |
| DevOps and CI/CD | <signal> | <evidence> | <guidance> | <placement> |
| Security and auth | <signal> | <evidence> | <guidance> | <placement> |
| Performance and optimization | <signal> | <evidence> | <guidance> | <placement> |

Unknown or thin signal is valid. Do not backfill missing evidence with generic assumptions.

## Optional Role Assessment

| Role | Signal strength | Evidence | Recommendation |
|------|----------------|----------|----------------|
| DevOps/CI-CD Specialist | <strong / moderate / thin> | <CI config, deployment scripts, containerization> | <generate / skip + rationale> |
| Security Specialist | <signal> | <auth modules, security config, vulnerability scanning> | <recommendation> |
| Performance Specialist | <signal> | <profiling, benchmarks, optimization patterns> | <recommendation> |
| API Designer | <signal> | <API definitions, schema files, versioning> | <recommendation> |
| Database Specialist | <signal> | <migrations, ORM config, query patterns> | <recommendation> |
| Documentation Specialist | <signal> | <doc generation, API docs, README quality> | <recommendation> |

## Domain Context

- glossary:
- business rules:
- lifecycle states:
- invariants:

## Business Knowledge Persistence Recommendation

- business complexity signal: <simple | moderate | complex>
- current business knowledge artifacts:
- recommended storage: <project context instruction only | domain-scoped instructions | business domain registry / domain map | business-domain skill + supporting instructions>
- rationale:
- agents that must consume it:
- what should remain summarized in project context instruction versus live in dedicated artifacts:

## Recommended Workspace-Level Instructions (`copilot-instructions.md`)

For the target project's `.github/copilot-instructions.md`:
- recommendation: <create from scratch | update existing>
- existing file quality assessment (if present):
- content to preserve from existing file (if any):
- content to add for agent ecosystem integration:
- concise project overview:
- technology stack summary:
- core conventions worth surfacing globally:
- agent ecosystem guide (available agents, primary prompts, how to invoke them):
- cross-references to deeper instruction files:

## Recommended Project Context Instruction Content

Content to include in the project's `<prefix>-project-context.instructions.md`:
- project overview:
- architecture summary:
- technology stack and defaults:
- key project-wide conventions:
- domain glossary:
- general coding standards:

## Project Complexity Assessment

- complexity level: <low / moderate / high / very high>
- rationale:

### Complexity Evidence

| Signal | Value | Implication for instruction detail |
|--------|-------|------------------------------------|
| Developer count | <solo / 1-2 / 3-8 / 8+> | <instruction detail note> |
| Module/target count | <count> | <instruction detail note> |
| Business domain complexity | <simple / moderate / complex> | <instruction detail note> |
| Process maturity | <minimal / some / formal> | <instruction detail note> |
| Existing agent ecosystem | <none / partial / established> | <instruction detail note> |

## Spec Pipeline Readiness

- recommendation: <skip | conditional activation | always-on>
- rationale:
- existing spec/PRD conventions:
- existing feature planning process:
- recurring under-specification issues:
- existing `specs/` or `docs/features/` directory:

## Review Pipeline Assessment

- recommendation: full 4-agent separated pipeline (Code Review Orchestrator + Functional + Technical + Platform/Framework)
- Platform/Framework Reviewer trigger conditions:
- current review methodology:
- functional/technical separation signal:
- platform/framework-specific review needs:
- typical change blast radius:

## Hooks Assessment

- linter detected: <yes/no, tool name, config location>
- formatter detected: <yes/no, tool name, config location>
- build system with clear default target: <yes/no, tool name>
- existing pre-commit hooks or CI checks:
- recommendation: <which hooks to generate / skip and why>

## Drift Summary (Established/Mixed Projects Only)

- total stale references found:
- severity: <cosmetic | misleading | harmful>
- details:

| File | What is stale | Current state | Recommended fix |
|------|--------------|---------------|-----------------|
| <file> | <stale reference> | <current state> | <fix> |

## Coverage Gaps And Conflict Risks

- missing role coverage:
- weak or redundant artifacts:
- naming inconsistencies:
- integration risks:

## Recommended Role Families

- <role family>

## Primitive Recommendations

- agents:
- skills:
- instructions:
- prompts:
- hooks:
- supporting templates or checklists:

## Generator Handoff Notes

- preserve:
- improve:
- avoid:
- micro-change lane guidance:
- reviewer conflict handling expectations:

## Risks And Missing Context

- <risk or missing fact>
