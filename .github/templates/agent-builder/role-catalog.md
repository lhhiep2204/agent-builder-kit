# Role Catalog

Use this as a default menu, not a mandatory output list.

> **Naming rule**: Generated dev workflow agents do NOT use the "Universal" prefix. That prefix is reserved for the Agent Builder Kit's own subagents (`Universal Copilot Docs Refresher`, `Universal Codebase Analyzer`, `Universal Agent Generator`, `Universal Quality Auditor`).

## Core Delivery Roles

- Business Analyst
- Investigator
- Implementor
- Test Specialist
- Dev Orchestrator

### Baseline Sufficiency Rule

These core delivery roles plus the separated review pipeline are the default baseline for full kits. Add extra specialist roles only when analyzer evidence shows that scale, risk, or coordination complexity cannot be handled efficiently by the baseline.

## Review Pipeline Roles

Always generate the full separated review pipeline:
- Code Review Orchestrator — routes to sub-reviewers, aggregates verdicts, enforces short-circuit
- Functional Reviewer — business correctness, AC traceability, edge cases
- Technical Reviewer — architecture, performance, security, layer responsibility
- Platform/Framework Reviewer — platform-specific and framework-specific checks (conditionally triggered at runtime when platform-specific or framework-specific files are changed)

### Platform/Framework Reviewer Trigger Examples

The analyzer determines the specific trigger conditions based on the project's technology stack:

| Technology | Trigger condition | Focus areas |
|-----------|-------------------|-------------|
| Swift/SwiftUI/UIKit | Swift or SwiftUI files changed | Memory management, actor isolation, UI thread safety, SwiftUI recomposition, accessibility |
| Android/Kotlin | Kotlin or Android XML files changed | Lifecycle management, memory leaks, coroutine safety, fragment/activity handling |
| React/Next.js | React component or Next.js files changed | Hydration, SSR/CSR consistency, hook rules, render performance |
| Spring/Java | Spring-annotated files changed | Bean lifecycle, dependency injection, transaction management, security config |
| Flutter/Dart | Dart widget files changed | Widget rebuild optimization, state management, platform channel safety |
| Rust | Rust files with unsafe blocks changed | Memory safety, lifetime correctness, concurrency (Send/Sync) |
| Go | Go files changed | Goroutine safety, channel patterns, error handling, context propagation |

## Spec Pipeline Roles

- Refine-User-Input — intake normalizer (skill, not a full agent)
- Specify-Feature — PRD generation (skill or agent depending on complexity)
- Plan-Implementation — architecture planning (skill)
- Generate-Tasks — task breakdown for execution tracking (skill)

These are typically skills, not standalone agents. Generate as agents only when the project's complexity warrants dedicated personas.

## Optional Expanded Roles

Generate these only when the analyzer detects strong project signal warranting them:

| Role | Generate when | Skip when |
|------|---------------|-----------|
| DevOps/CI-CD Specialist | Complex CI/CD pipelines, multi-environment deployments, containerization, IaC | Simple CI, single deployment target |
| Security Specialist | Auth modules, security-sensitive data handling, compliance requirements | No auth, internal-only tool |
| Performance Specialist | Performance-critical code, benchmarks, profiling infrastructure | Standard CRUD, no perf requirements |
| API Designer | Public APIs, schema-first development, API versioning | Internal APIs only, simple REST |
| Database Specialist | Complex migrations, multiple data stores, query optimization needs | Simple ORM, single DB |
| Documentation Specialist | Public-facing docs, API documentation generation, doc-as-code | Internal project, README-only |

## Ecosystem Roles

- Agent Ecosystem Auditor
- Agent Integration Specialist

## Specialized Quality Roles

- Architect
- Performance And Concurrency Reviewer
- Accessibility And Localization Reviewer
- Release Readiness Reviewer
- Migration And Refactoring Specialist

### Large-Task Scale-Up Hints

- Use `Architect` when architecture-level sequencing or boundary governance is the main risk.
- Use `Migration And Refactoring Specialist` when multi-phase migrations dominate delivery risk.
- Use `Release Readiness Reviewer` when integration and rollout safety dominate delivery risk.

## Generation Rule

Generate a role only when it changes execution quality materially. Do not create a role just because the catalog makes it available. Every Full Kit bundle includes the full core delivery roles, separated review pipeline, and spec pipeline skills. Specialized quality roles and optional expanded roles are added when the project's complexity and domain warrant them.
