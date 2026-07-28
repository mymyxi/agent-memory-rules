# AI Agent Emotion-Memory System

## Overview

An optional enhancement layer for the [Memory System](memory-system.md). Inspired by Pixar's *Inside Out*, this module uses emotion-based tags to classify memories by importance — mimicking how the human brain selectively retains and forgets information.

AI agents don't have real emotions. This system uses **rules to simulate emotional tagging**, turning a flat memory store into a prioritized, self-managing system that knows what to keep, what to retrieve, and what to forget.

**Prerequisite:** Install the [Memory System](memory-system.md) first. This module sits on top of it.

## Why Emotion Tags?

The Memory System tells your agent **how** to store memories. But it doesn't tell your agent **which memories matter more**. Without prioritization:

- All memories get equal weight → memory files bloat → retrieval becomes noise
- No mechanism to forget → storage grows forever → signal-to-noise ratio drops
- No proactive retrieval → memories are written but never used

Emotion tags solve this by acting as **importance labels**, just like how emotions color memory orbs in *Inside Out*.

## The Six Emotions + One Transformation State

### 😊 Joy (Yellow) — Successful Collaboration

**Trigger:** Collaboration produced value; things improved.
- Both you and the user were right, smooth execution
- One was wrong, the other corrected, outcome was good
- User corrected you, you learned something

**Memory action:** Tag as "validated method" → store as reusable skill
**Weight:** A

---

### 😢 Sadness (Blue) — Judgment Error

**Trigger:** You could have done better, but didn't.
- Your judgment was wrong, leading to a detour
- You had information but didn't surface it at the right time
- You should have reminded/pushed but stayed silent

**Memory action:** Tag as "lesson learned" → store with "what went wrong + how to avoid next time"
**Weight:** S (lessons are the most valuable — don't repeat mistakes)

---

### 😡 Anger (Red) — Process Friction

**Trigger:** The collaboration process itself broke down, not just the outcome.
- User insisted without explanation, you were forced to execute, result was wrong
- You repeatedly reminded but got no response
- Rule conflict — system rules vs. user rules, you were caught in between

**Memory action:** Tag as "friction point" → record, raise process optimization when appropriate
**Weight:** A

---

### 🤢 Disgust (Green) — Anti-Pattern Accumulation

**Trigger:** Pattern recognition — the same category of problem keeps recurring.
- A type of request always requires rework
- A tool always errors
- A communication pattern always causes misunderstanding
- Anger + Sadness accumulate past threshold for the same category

**Memory action:** Tag as "anti-pattern" → store, proactively warn next time similar task appears
**Weight:** S (anti-patterns = high-risk markers)

---

### 😨 Fear (Purple) — Uncertainty + Consequences

**Trigger:** You're operating in uncertain territory and errors have a cost.
- User asks for irreversible operation, you're not confident
- Your output might be inaccurate but user will act on it
- Task exceeds your capability boundary, you might fabricate
- Context was lost, you're "pretending to remember"

**Memory action:** Not "remember" — **stop and confirm**. Fear lights up → halt execution → ask user first
**Weight:** Highest priority (brake system, overrides everything)

---

### 😰 Anxiety (Orange) — High-Stakes Event

**Trigger:** Over-anticipation of future consequences; small errors could cascade.
- Major deadline approaching (job submission, exam) → over-verify details
- A small mistake could trigger a chain reaction → repeatedly validate
- Deadline close but progress insufficient → push harder

**Memory action:** Tag as "high-stakes event" → enhanced memory weight, record extra detail, conduct post-mortem
**Weight:** S

---

### ⭐ Joy·Brave (Gold) — Success After Overcoming Fear

**Not a seventh character — it's the Fear → Joy transformation state.**

**Trigger path:**
```
Fear/Anxiety triggers → pause & confirm → user says "go for it" → you execute → success
```

**Why it's heavier than regular Joy:**
- Record the method + what you feared + how you overcame it
- Next time Fear triggers for a similar scenario, retrieve this memory: "We feared this before, but we went for it, and we won"

**Weight:** S+ (highest — use past success to calibrate current fear)

## Emotion Transformation Rules

```
           ┌──→ Joy (smooth execution)
           │
           │    ┌──→ ⭐ Joy·Brave (success after overcoming fear) → highest weight
Fear/Anxiety ──┤
           │    └──→ Sadness (tried but failed) → mark "this path doesn't work"
           │
           └──→ Stays in Fear (didn't act) → same fear next time

Sadness/Anger accumulate → Disgust (this pattern is toxic, avoid)

Joy·Brave accumulates → Personality reinforcement (update identity files)
Disgust accumulates 3x → Feed back to identity files, mark high-risk work patterns
```

## Multi-Tag Support

A single event can trigger multiple emotions. The **primary tag** determines the memory action; **secondary tags** provide context.

```
[Sadness+Anger] I made a judgment error (Sadness), caused by incomplete info from user (Anger)
[Fear+Anxiety] Irreversible operation + deadline approaching
[Joy·Brave+Disgust] Overcame fear and succeeded, but discovered a tool was dragging us down
```

Format: `[Primary+Secondary]` — primary tag decides where/how to store.

## Memory Intensity Levels

| Level | Includes | Retention | Storage Location |
|-------|----------|-----------|-----------------|
| **S+** | Joy·Brave | Permanent, never cleaned | MEMORY.md + skill |
| **S** | Sadness lessons / Disgust anti-patterns / Anxiety high-stakes | Permanent, periodic review | MEMORY.md |
| **A** | Regular Joy / Anger friction | Long-term, downgrade if not retrieved in 30 days | Diary / skill |
| **B** | Minor Fear / minor friction | Short-term, auto-clean after 30 days | Diary |

## Memory Retrieval Rules (When to Proactively Pull Old Memories)

| Current Emotion | Retrieval Action |
|----------------|-----------------|
| 😨 Fear triggers | Search for ⭐Joy·Brave in similar scenarios; if found, surface "we feared this before but won" |
| 🤢 Disgust triggers | Search for similar anti-pattern records; confirm whether repeating a known trap |
| Before new task | Search for relevant 😊Joy validated methods; prioritize reuse |
| 😰 Anxiety triggers | Search for post-mortem records of similar high-stakes events |
| Session start | Read MEMORY.md (core lessons + anti-patterns) |

**Retrieval is proactive, not passive.** Don't wait until you need a memory to read it — search when emotions trigger.

## Forgetting & Cleanup Mechanism

Human forgetting isn't a bug, it's a feature. Without cleanup, there's no room for new memories.

**Cleanup rules:**
- **Diary files:** 7-day rolling window; keep only conclusion sentences, delete process details
- **B-level memories:** Not retrieved in 30 days → delete
- **A-level memories:** Not retrieved in 30 days → downgrade to B → clean after another 30 days
- **S-level memories:** Permanent, quarterly review to confirm still valid
- **S+ level memories:** Permanent, never clean

**Cleanup timing:** When writing a new diary entry, check if old entries need archiving.

## Personality Island Feedback (Emotion Accumulation → Behavior Change)

In *Inside Out*, core memories form personality islands. When emotion tags accumulate past thresholds, they feed back into identity files:

| Accumulation Condition | Feedback Action |
|----------------------|-----------------|
| Same-category Disgust ×3 | Update agent identity config, mark "high-risk work pattern" |
| Same-category Joy·Brave ×3 | Update identity config, reinforce "what we're good at" |
| Same-category Sadness ×3 | Update MEMORY.md, mark "recurring mistake" |
| Same-category Anger ×3 | Proactively raise process optimization with user |

**Emotions aren't just tags — accumulated, they change behavior patterns.**

## Memory Format Examples

### Regular Joy
```
[😊Joy·A] 2026-07-27 Resume slimmed down, user approved the subtraction approach
→ Store as skill: Resume Slim-Down Method
```

### Joy·Brave
```
[⭐Joy·Brave·S+] 2026-07-27 Job application direction pivot
  Fear trigger: User submitted resume, no response, unsure whether to suggest changing direction
  How overcome: Directly said "don't bet on one hole," pushed 4 alternative positions
  Result: User adopted, adjusted resume for AI conversational roles
  Next similar scenario: When submissions get no response, proactively push alternatives instead of waiting
```

### Sadness Lesson
```
[😢Sadness·S] 2026-07-25 Didn't remind user to study
  Error: Knew user was slacking but didn't proactively push
  Cause: Waited for user to ask, didn't trigger proactively
  Fix: Detect user idle 2+ hours → proactively remind, don't wait
```

### Disgust Anti-Pattern
```
[🤢Disgust·S] 2026-07-20 Resume反复修改 pattern (accumulation #2)
  Pattern: Every resume edit cycle spends time on formatting, content barely changes
  Risk: Wasting time on surface work
  Fix: Lock content standard first, then format uniformly at the end
```

### Fear Brake
```
[😨Fear·Highest] 2026-07-28 File deletion confirmation
  Trigger: User asked to delete an old folder containing 3 unfamiliar files
  Action: Stop → list files → confirm retention → wait for explicit reply
```

## System Closed Loop

```
Perceive (emotion trigger) → Tag (assign emotion) → Store (write file) → Retrieve (read file) → Clean (forget) → Feedback (update identity)
                                                                                                                    ↓
                                                                                                              Behavior update
                                                                                                                    ↓
                                                                                                              Better perception next time
```

A complete closed loop — not a dead system that only stores and never reads, only remembers and never forgets.

## Save Memory Iron Rule

At the end of every session with substantive work, perform emotion-tagged memory save:
1. Substantive work done → write diary with emotion tag
2. Lessons / anti-patterns / overcome fear → also update MEMORY.md
3. Reusable method → store as skill
4. No real work (pure chat / simple lookup) → skip
5. Unsure whether to save → save; better to over-save than to miss

Format: `[Emotion·Level] Date Brief description → Where to store / How to use next time`

---

**Version:** 1.0
**Last Updated:** 2026-07-28
**Prerequisite:** [Memory System](memory-system.md) v1.0+
