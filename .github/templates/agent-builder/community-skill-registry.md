# Community Skill Registry

> Persistent snapshot of known community agent skill repositories organized by technology area.
> The analyzer reads this as a baseline during generation. Updated by `Universal Copilot Docs Refresher` during kit maintenance.

## Metadata

| Field | Value |
|-------|-------|
| Last refreshed | *not yet refreshed* |
| Source repos | *(add during first refresh)* |
| Total categories | 0 |
| Total skills | 0 |

## How This File Is Used

1. **Analyzer reads** this registry after determining the Technology Alignment Profile
2. **Matches** project tech stack against category tags below
3. **Deep crawls** matching skill repos via MCP GitHub when available
4. **Generator embeds** extracted patterns, anti-patterns, and rules into generated agents
5. **Staleness check**: if Last refreshed > 30 days ago, analyzer notes staleness in output

## Fallback Chain

| Priority | Method | Coverage |
|----------|--------|----------|
| 1 | MCP live + this registry | Full: live content + baseline categories |
| 2 | Web fetch + this registry | Good: fetched content + baseline categories |
| 3 | This registry only | Acceptable: pre-extracted summaries only |

Never skip community skill discovery entirely — this registry ensures baseline coverage even without network access.

## Registry Format

Each entry follows this structure:

```
### <Category Name>
- **Tags**: <comma-separated technology tags for matching>
- **Repo**: <owner/repo>
- **Content path**: <path to SKILL.md or main content file>
- **Match criteria**: <when to include this skill in generation>
- **Extracted knowledge**: <key patterns, anti-patterns, rules extracted from the skill>
```

## Categories

*(Empty — populate during first kit maintenance refresh using `Universal Copilot Docs Refresher`)*

<!-- To add a new category:
1. Identify the community skill repository
2. Read its SKILL.md or main content file
3. Extract key patterns, anti-patterns, and rules
4. Add an entry following the Registry Format above
5. Update the Metadata table counts
-->
