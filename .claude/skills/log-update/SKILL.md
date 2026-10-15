---
name: log-update
description: Maintain PROJECT_LOG.md, the single source of truth about where the project stands, so any agent or person can continue without the chat history. Use at the end of a work session or significant task, when a major decision is made, before another agent takes over, or when the user asks to log or update the log.
---

# log-update

## Purpose

Maintain `PROJECT_LOG.md` — a structured work log that enables any AI agent (or human) to understand the current project state and continue work without needing the full chat history.

## When to invoke

- At the end of a work session or significant task
- When a major decision is made
- When another agent is about to take over
- When the user asks to "log", "update the log", or "document progress"
- Automatically: after completing any task that changes files, architecture, or project direction

## Log file structure

The log file `PROJECT_LOG.md` always follows this exact structure:

```markdown
# PROJECT_LOG.md

_Last updated: YYYY-MM-DD HH:MM_
_Updated by: [agent, environment and model, as the tool names them — e.g. "Claude (Cowork, Opus)"]_

---

## CURRENT STATE

[2-5 sentence summary of where the project stands RIGHT NOW.
What is working, what is in progress, what is blocked.
This section is rewritten on every update — it is never appended.]

## ACTIVE TASKS

| # | Status | Task | Notes |
|---|--------|------|-------|
| 3 | 🔄 IN PROGRESS | task description | brief note |
| 4 | ⏳ PENDING | task description | brief note |
| 1 | ✅ DONE | task description | completed on date |

## NEXT SESSION

[The plan for the next session: the owner's tasks, then the agent's tasks grouped by what
they block, each with its task number, where to run it and which model and thinking level to use.
Written by the `pause-update` skill and rewritten in full at every pause.
Until the first pause: "No pause yet."]

## KEY DECISIONS

[Numbered list of important decisions made, with brief rationale.
Append new decisions — do not remove old ones. Decisions are numbered D1, D2, … — a counter
of their own, separate from task numbers.]

D1. YYYY-MM-DD — Decision description. Reason: ...
D2. ...

## ACTIVE SKILLS

Skills currently installed in `.claude/skills/`:
- `skill-name` — why it was installed

## HISTORY

[Chronological append-only log. Each entry is a brief paragraph.
Never edit past entries. Always add new entries at the top of this section.]

### YYYY-MM-DD — Session summary title

What was done, what changed, what was learned. Key files modified: ...

---
```

## Protocol

### Reading the log

Before starting work on any task, read `PROJECT_LOG.md` if it exists. Focus on `CURRENT STATE`, `NEXT SESSION` and `ACTIVE TASKS` — this tells you where to start.

### Updating the log

1. **Rewrite `CURRENT STATE`** — replace entirely with fresh summary
2. **Update `ACTIVE TASKS`** — add/update/mark done
3. **Append to `KEY DECISIONS`** — only if a significant decision was made
4. **Update `ACTIVE SKILLS`** — reflect current `.claude/skills/` contents
5. **Prepend to `HISTORY`** — add a new entry at the TOP of the history section

### Creating a new log

If `PROJECT_LOG.md` does not exist, create it using the template above with:
- Current state as "Project initialized"
- Empty tasks and decisions
- First history entry describing project setup

### Compression (when history grows large)

When the `HISTORY` section exceeds ~50 entries:
1. Summarize entries older than 30 days into a single compressed entry
2. Prefix: `[COMPRESSED] YYYY-MM-DD to YYYY-MM-DD — summary`
3. Keep the last 20 individual entries intact

## Important rules

- `CURRENT STATE` is always current — never leave stale information there
- Dates and times (`Last updated`, `HISTORY` headings) are read from the system clock when written — never estimated, **never typed**. The time must reach the file from the clock's output, not through you: either the script that writes the entry reads the clock itself, or you write a placeholder (such as `__TIME__`) and the same command that reads the clock puts its output in its place (in a POSIX shell: `T=$(date "+%Y-%m-%d %H:%M")`, then replace the placeholder with `$T`). Afterwards search the file: no placeholder may be left. If neither is possible, save first and take the label from the file's modification time. A time you remember from earlier in the conversation, or one shown at the top of the owner's message, is an estimate
  > why: the rule "from the system clock" was broken in two test sessions in a row — once with an invented time, once with an estimate one minute off. Both times the agent knew the rule; it had just read the clock earlier and wrote from memory. The next fix, reading the clock in the same command that writes, was not enough either: in a real project the agent put the clock and the write in one command, as the rule said, and still wrote 20:09 while the clock printed 20:07 — it typed the number before it saw the output. Seven misses in two sessions; once the command substituted the clock's output itself, none for the rest of the session. A clock attached to every message didn't help in the middle of a long answer: by the time the answer writes, that time is minutes old.
- The same goes for a time **inside the text** of an entry ("checked at 20:31"): copy it from the output that printed it — the clock, a commit, a tool's log line — or leave it out
  > why: in a later test session the agent wrote "at 20:31" into a `HISTORY` entry while the clock showed 20:28 — the time it expected the step to end, not one it had read. The rule about labels held; the time in the sentence was nobody's label.
- Tasks and decisions are **two separate counters**. A decision is always written with its `D` — in the log, the plan and the chat — so "20" can only mean task 20
  > why: in two test sessions in a row the counters got mixed. Once the agent skipped a task number because a decision already had it; the next day task 20 and decision 20 were two different things, and "blocks 20" in the briefing could be read either way.
- Every task keeps **one number** from the day it is added until it is done — in `ACTIVE TASKS`, `NEXT SESSION`, the chat and the archive. A new task gets the next unused number; a number is never reused or given to another task, and nothing is renumbered when a task is added or finished
- `HISTORY` is append-only — never delete or edit past entries
- Be concise — every section should be scannable in under 30 seconds
- Include file paths when referencing modified files
- If work was done by a subagent or external tool, note it in history
