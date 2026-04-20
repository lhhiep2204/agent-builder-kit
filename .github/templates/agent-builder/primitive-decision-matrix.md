# Primitive Decision Matrix

Use this matrix when deciding which Copilot customization primitive to use for a given behavior. Each row is a behavioral need; the columns are the available primitives.

## Decision Flow

```
Is the behavior always relevant regardless of context?
  YES → Instruction (repo-wide or path-specific)
  NO  →
    Is it triggered by a user command / entry point?
      YES → Prompt
      NO  →
        Is it a role with tools and subagent orchestration?
          YES → Agent
          NO  →
            Is it a bundle of detailed guidance loaded on-demand by description match?
              YES → Skill
              NO  →
                Is it a deterministic gate that must block or allow a tool call?
                  YES → Hook
                  NO  → Instruction (narrowly scoped)
```

## Matrix

| Behavioral Need | Instruction | Skill | Agent | Prompt | Hook |
|-----------------|:-----------:|:-----:|:-----:|:------:|:----:|
| Always-on convention (naming, style, patterns) | ✅ | | | | |
| Path-specific rule (test files, config files) | ✅ | | | | |
| On-demand detailed guidance (complex workflow) | | ✅ | | | |
| Role with tool access and orchestration | | | ✅ | | |
| User-facing entry point / command | | | | ✅ | |
| Deterministic gate (block dangerous tool call) | | | | | ✅ |
| Build/test validation after code generation | ✅ | | | | |
| Domain knowledge reference (API patterns, arch) | | ✅ | | | |
| Multi-step workflow coordination | | | ✅ | | |
| One-shot task with structured output | | | | ✅ | |

## Technology Split Rule

When a behavior spans multiple primitives, apply this rule:

| Aspect | Primitive |
|--------|-----------|
| **What** the project does (facts, conventions, stack) | Instruction |
| **How** to do complex work (detailed steps, templates) | Skill |
| **Who** does the work (role, tools, delegation) | Agent |
| **When** the user triggers it (entry point) | Prompt |
| **Whether** a tool call is allowed (gate) | Hook |

## Common Anti-Patterns

| Anti-Pattern | Why It's Wrong | Fix |
|-------------|---------------|-----|
| Encoding conventions in an agent body | Agent bodies should define roles, not repeat rules | Move to instruction |
| Using a hook for advisory guidance | Hooks are gates, not suggestions | Move to instruction |
| Putting entry-point logic in a skill | Skills are loaded by relevance, not user command | Move to prompt |
| Creating a monolithic skill | Large skills dilute relevance matching | Split into focused skills |
| Duplicating rules across agents | Creates drift when rules change | Extract to shared instruction |
