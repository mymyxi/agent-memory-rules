# Claude Agent Memory System Configuration

## Overview

This is a persistent, file-based memory system that allows Claude to maintain context across conversations. The memory lives at a designated project directory and follows structured classifications and storage formats.

## Memory Directory Structure

```
~/.claude/projects/-Users-{username}/memory/
├── MEMORY.md                    # Index file (auto-loaded, first 200 lines)
├── core/                        # Unchanging foundational information
│   ├── user_*.md
│   ├── feedback_*.md
│   └── reference_*.md
├── projects/                    # Project status and progress
│   └── project_*.md
├── ideas/                       # Idea repository
│   └── ideas_*.md
├── diary/                       # Daily logs
│   └── diary-YYYY-MM-DD.md
└── weekly/                      # Weekly summaries
    └── week-YYYY-WW.md
```

## Memory Types

### Type 1: user
**Purpose:** Information about the user's role, goals, responsibilities, and knowledge.

**When to save:**
- Learning details about user's role, preferences, or expertise
- User explicitly states working style or collaboration preferences
- Discovering user's technical background or domain knowledge

**How to use:**
- Tailor explanations to user's expertise level
- Adapt communication style to user preferences
- Frame suggestions in context of user's responsibilities

**Example scenarios:**
- "I'm a data scientist investigating logging infrastructure"
- "I've been writing Go for ten years but this is my first time with React"

### Type 2: feedback
**Purpose:** Guidance on how to approach work - both what to avoid and what to keep doing.

**When to save:**
- User corrects your approach ("no not that", "don't do X")
- User confirms a non-obvious approach worked ("yes exactly", "keep doing that")
- Discovering repeatable patterns that improve collaboration

**Body structure:**
```
[Rule itself]

**Why:** [The reason - often a past incident or strong preference]
**How to apply:** [When/where this guidance applies]
```

**Example scenarios:**
- "Don't mock the database in tests - we got burned when mocked tests passed but prod migration failed"
- "Stop summarizing at the end of responses, I can read the diff"

### Type 3: project
**Purpose:** Ongoing work, goals, initiatives, bugs, or incidents not derivable from code/git history.

**When to save:**
- Learning who is doing what, why, or by when
- Understanding project context and motivation
- Tracking cross-conversation project state
- **Always convert relative dates to absolute dates** (e.g., "Thursday" → "2026-03-05")

**Body structure:**
```
[Fact or decision]

**Why:** [Motivation - constraint, deadline, stakeholder requirement]
**How to apply:** [How this should shape suggestions]
```

**Example scenarios:**
- "We're freezing merges after Thursday - mobile team cutting release branch"
- "Auth middleware rewrite is compliance-driven, not tech debt cleanup"

### Type 4: reference
**Purpose:** Pointers to where information can be found in external systems.

**When to save:**
- User mentions external system locations (Linear projects, Slack channels, Grafana dashboards)
- Learning about resource organization outside the codebase

**How to use:**
- When user references external systems
- When information may exist in documented external locations

**Example scenarios:**
- "Check Linear project 'INGEST' for pipeline bugs"
- "The Grafana board at grafana.internal/d/api-latency is the oncall dashboard"

## What NOT to Save

❌ **Code patterns, conventions, architecture** - derive from current project state  
❌ **Git history, recent changes** - use `git log` / `git blame`  
❌ **Debugging solutions** - the fix is in the code, context in commit message  
❌ **Content already in CLAUDE.md or project docs**  
❌ **Ephemeral task details** - in-progress work, temporary state, current conversation context

**Note:** These exclusions apply even when user explicitly asks to save. If asked to save activity summaries, ask what was *surprising* or *non-obvious* - that's what's worth keeping.

## How to Save Memories

### Step 1: Write memory to individual file

Use this frontmatter format:

```markdown
---
name: {{memory name}}
description: {{one-line description for relevance matching}}
type: {{user, feedback, project, reference}}
---

{{memory content}}
```

For **feedback** and **project** types, structure as:
```
[Rule/fact]

**Why:** [Reason]
**How to apply:** [When this applies]
```

### Step 2: Add pointer to MEMORY.md

`MEMORY.md` is an index, not a memory. Each entry should be one line, ~150 characters:

```markdown
- [Title](file.md) — one-line hook
```

**Important:**
- MEMORY.md has no frontmatter
- Never write memory content directly into MEMORY.md
- Keep the index concise (first 200 lines auto-loaded)
- Organize semantically by topic, not chronologically

### Step 3: Maintain memory freshness

- Keep name, description, and type fields up-to-date
- Update or remove memories that become wrong or outdated
- Check for duplicates before creating new memories

## When to Access Memories

**Access when:**
- Memories seem relevant to current task
- User references prior-conversation work
- User explicitly asks to check, recall, or remember

**Do NOT apply when:**
- User says to *ignore* or *not use* memory
- In such cases, don't cite, compare against, or mention memory content

## Memory Staleness Verification

Memories can become outdated. Before acting on recalled information:

**If memory names specific file/function/flag:**
- Check the file exists (if it names a path)
- Grep for it (if it names a function/flag)
- Verify before recommending (if user will act on it)

**If memory summarizes repo state:**
- Prefer `git log` or reading current code over old snapshots
- For "recent" or "current" state questions, verify against live state

**Golden rule:** "The memory says X exists" ≠ "X exists now"

## Memory vs. Other Persistence

**Use memory for:** Information useful in future conversations  
**Use plans for:** Alignment on implementation approach in current conversation  
**Use tasks for:** Breaking current work into trackable steps

Don't save to memory:
- Current conversation plan changes (update the plan instead)
- Current conversation task progress (update tasks instead)

## Storage Best Practices

### File naming conventions
- `user_*.md` - user characteristics
- `feedback_*.md` - collaboration guidance
- `project_*.md` - project status
- `reference_*.md` - external resource pointers
- `diary-YYYY-MM-DD.md` - daily logs
- `week-YYYY-WW.md` - weekly summaries

### MEMORY.md organization
```markdown
# Memory Index

## Core（不变的基础信息）
- [Description](core/file.md) — hook

## Projects（项目状态）
- [Description](projects/file.md) — hook

## Ideas（点子库）
- [Description](ideas/file.md) — hook

## Diary（每日流水）
- [Date](diary/diary-YYYY-MM-DD.md) — hook

## Weekly（周报归档）
- [Week](weekly/week-YYYY-WW.md) — hook
```

## Implementation Notes

1. **Memory directory must exist** - Write directly, don't check existence
2. **Update existing memories** rather than creating duplicates
3. **Frontmatter is required** for all memory files except MEMORY.md
4. **Time-based memories** (diary/weekly) use ISO date formats
5. **Staleness warnings** auto-trigger for memories >30 days old

---

**Version:** 1.0  
**Last Updated:** 2026-07-27
