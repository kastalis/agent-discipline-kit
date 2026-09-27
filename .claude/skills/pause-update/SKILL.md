---
name: pause-update
description: When the user calls a pause or a session ends, bring all living project documents up to date in one pass - run log-update, update only the docs this session changed, verify no file is truncated, and write a dated briefing with who does what next. Use immediately on "pause", "let's stop here", "we're done for now", or the same in the user's language.
---

# pause-update

## Purpose

When the user calls a pause (or a work session ends), bring ALL living project documentation up to date in a single pass, so any agent or human can resume cleanly from the docs alone — without the chat history. This is a **superset of `log-update`**: it always runs the log-update protocol for `PROJECT_LOG.md`, then reviews every other living doc and updates only those this session actually changed, syncs memory, produces a **pause briefing** with an explicit division of labour for what comes next, and finally verifies that every edited file is intact (not truncated).

## When to invoke

- **IMMEDIATELY** when the user signals a pause or stop. Trigger phrases (non-exhaustive): "let's pause", "we're done for now", "stop here", "that's it for today" — or the equivalent phrase in the user's language.
- At the end of any meaningful work session, and before handing off to another agent.
- The user should **not** need to separately ask to "update the md files" — the pause is the trigger. Treat the pause request itself as the instruction to run this whole procedure.

## Procedure

Run the steps in order. Read the **system date and time** for every new date or time label, in the same step that writes it — never estimate a time, and never reuse one you read earlier (see `log-update`).

### 1. PROJECT_LOG.md — always (run `log-update`)

Follow the `log-update` protocol exactly:

- Rewrite **CURRENT STATE** (replace entirely; never leave stale info).
- Update **ACTIVE TASKS** (add new, mark done, adjust notes).
- Append to **KEY DECISIONS** only if a genuinely new decision was made (append-only).
- Refresh **ACTIVE SKILLS** to match the real `.claude/skills/` contents.
- Prepend ONE new **HISTORY** entry at the top (append-only; never edit past entries). Name the key files touched.

### 2. Other living docs — update ONLY what changed

Go through the project's other living documents — those listed in `AGENTS.md` → RESOURCES, plus any file modified this session — and for each ask: *did this session change something this doc is responsible for?* Update it only if yes; otherwise leave it untouched.

"Update all md files" does **not** mean rewrite everything — it means touch only the ones this session affected. You decide which; the user need not enumerate them.

Typical docs and their triggers (adapt to the project at hand):

- An **editor/tool "map"** (how to work in tool X) — if any tool/browser/editor technique was used or learned.
- **Schema / data-model docs** — if tables, columns, types, or validation changed.
- A **tool-agnostic PRD/spec** — if a product-level requirement or decision changed.
- **How-to / build guides** — if working steps or expressions changed.
- **`AGENTS.md`** — if project config, conventions, constraints, or RESOURCES changed.
- **Tooling / roadmap docs** — if the tooling plan changed.

### 3. Memory (cross-session facts)

Check whether any durable, non-obvious fact emerged (user feedback, a project constraint, a reusable technique). If so, store it wherever your agent keeps memory between sessions — a memory file plus its index entry, if your agent uses files for that. If the environment keeps its own memory and gives you no way to write it, skip this step and make sure the fact is in the log instead. Don't duplicate what the repo/docs already capture.

The other direction matters as much: if this session **overturned** something you remembered, correct or delete it in memory. If you can't write memory, put it on the owner's task list in `NEXT SESSION` (and so in the briefing) as a task of its own: which claim is wrong, what is true instead, and where that memory lives, so the owner can remove it. A memory that was proven wrong and left in place will be repeated in the next session's first reply. Say in the briefing that this step was done, even when there was nothing to store.

### 4. Integrity verification (truncation and leftovers)

For **every** file edited in steps 1–3, read the **whole** file back — not only its end — and check two things:

1. **It ends cleanly** — no mid-sentence cutoff, closing structure present. This guards against the truncated-file failure mode that happens after session crashes, especially in projects without git or backup. If a file is truncated, repair it from intact sources before reporting.
2. **Nothing was left to fill in later** — search it for markers such as `__`, `TODO`, `TBD`, `XXX`, `(fill in)`, `YYYY-MM-DD`, a name still in curly braces (`{date}`, `{now[:10]}` — code that wrote its template instead of its value), or a template line that still has its placeholder text. A marker you meant to replace at the end is the step that gets lost.

> why: in a test session the agent left a placeholder in the middle of the session log, meaning to replace it with its closing message, and never did. It then reported that it had read every file back and each "ends cleanly" — which was true, and missed the gap, because the check only looked at the end.

### 5. Pause briefing — state, division of labour, what's next

Every pause produces a briefing in **three** places:

1. **The chat** — for the user now. **Write it from the log, not separately**: the tasks in the
   chat are the tasks in `NEXT SESSION`, the same ones in the same order. A task that is in one
   and missing from the other is an error, not a summary.
2. **The log** — `## NEXT SESSION`, placed directly after `ACTIVE TASKS`, rewritten in full
   each pause the way `CURRENT STATE` is. This is the authoritative copy: the log must stay
   self-sufficient, because it is the only bridge between environments.
3. **A dated snapshot** — `archive/pause-YYYY-MM-DD-HHMM.md`, holding the same briefing plus
   the reconciliation described below. **Write-once**: once saved, an archive file is never
   edited, exactly like a `HISTORY` entry.

The snapshot is not redundancy for its own sake. `NEXT SESSION` is rewritten in full at every
pause, so the *previous* plan is destroyed the moment a new one is written — and with it any
way to ask whether the last plan was any good. The archive is what makes that question
answerable.

**Therefore every archive file opens with a reconciliation** against the previous archive
file (the newest one in `archive/`, if any). Three short lists, no prose padding:

- **Done** — items from the last briefing that are now done
- **Not done** — items that are not, each with the reason in one clause
- **Not in the plan** — work this session did that no one had planned, and why it came up

If the previous plan was largely missed, say so plainly; a planning loop that only records
successes measures nothing.

**Reading order at the start of a session** stays: newest `archive/*.md` (cheap orientation,
one minute) → `PROJECT_LOG.md` (`CURRENT STATE`) → `AGENTS.md` → the doc for the task at hand.
The archive **never replaces** the log: it carries the *what next*, the log carries the *why*.

The briefing has four parts, in this order:

1. **Where the project stands** — the whole arc, not just today: what is built and working, what
   is half-built, what has not been started. Short enough to read in a minute.
2. **What this session did** — concrete outcomes, not activity. Name what changed and what it
   means. Include what was attempted and failed, and any earlier claim this session overturned.
3. **The user's tasks — offline only.** Only things that genuinely require a human: decisions,
   account and payment actions, anything behind 2FA, physical or legal steps, subjective
   judgement (listening, watching, approving). Each with why it is blocking and roughly how long
   it takes. **Never put on this list work the agent can do itself** — if it lands here, justify
   why in one clause.
4. **The agent's tasks for the next session, grouped by what they block, with the
   environment marked per item.** Group by consequence, not by tool: what blocks the next
   deliverable comes first, then dated/measurement work, then everything that blocks nothing.
   Order within each group by dependency, and mark which items are blocked by a user task.

   **Every item also carries an environment mark** using the project's own split (defined in the
   project's `AGENTS.md` if it has one; a simple default is C = chat/desktop environment with
   connectors, K = terminal/coding environment, M = the owner only). The mark is **advice, not a restriction** — either environment
   may execute an item marked for the other. It exists because the user chooses the environment
   *before* the session starts and needs to know which one suits the work that is next.

   Why grouping and marking are separate (learned 2026-09-05): grouping by environment was
   tried and failed — for five consecutive pauses not a single item from one group was done,
   because a group is not a priority. Dropping the mark altogether then failed too, for the
   opposite reason: it removed the one piece of information the user needed before the session
   began. Group by blocking; mark by environment.

   **Every item for an agent also carries a model and a thinking level**, written the way the
   owner's tool names them — the model names from its model picker, and its own effort or
   thinking levels (for example `Opus · high`). Don't translate them into generic tiers: the
   owner picks the model from that same picker before the session starts, and a name they
   recognise is one they will actually use. Pick the level by **what an error would cost, not by
   how long the work is**: a status check or one command gets the lightest level; mechanical
   work following a recipe in the log gets a medium one; code, production and writing get a high
   one; verdicts, statistics and study design get the highest one normally used. Put the
   strongest model only where a mistake costs more than the tokens — on a plan with usage
   limits, the heavier model uses up the limit faster. If the tool offers no choice of model,
   leave the mark out rather than invent one. If you know your own model but can't see the
   picker, don't guess the level names: ask the owner once, write the names into `AGENTS.md` →
   `CONVENTIONS`, and take them from there at every later pause.

   Why: without the mark the owner opens the next session on whatever model is selected by
   default and has no way to know whether that was the right one for the work that is next.

**Every item carries its task number**, the one it got when it was added (see `log-update`).
A new item gets the next unused number; nothing is renumbered when an item is added, finished or
moved to another group. Group letters or headings say how urgent an item is — they are not its
name, so "depends on A1" is never written; "depends on 12" is.

> why: in a test session "A1" meant one task during the session and another after the pause, and
> a row in the log said "depends on A1". Adding one task the same evening renumbered three others,
> so the plan in the log no longer matched the write-once snapshot taken less than an hour before.

Keep every item actionable: what to do, where, and what "done" looks like. If an item exists only
because something is unknown, say what measurement would settle it.

### 6. Confirm

Briefly tell the user which files were updated and that all are saved and verified intact — both checks from step 4, not only the first.

If the owner asked for the closing message itself to be saved in a file (a session transcript, for example), **write the message into the file first, verify the file (step 4), then send the same text** to the chat. Don't send it and plan to paste it in afterwards: after the message is sent, the turn is over.

## Important rules

- The **pause itself** is the trigger — don't wait for a separate "update the docs" request.
- `HISTORY` and `KEY DECISIONS` are **append-only** — never edit or delete past entries.
- Update **only what changed**; never churn unchanged files.
- Always **verify integrity by reading back** the whole file (truncation after a crash is silent, and so is a placeholder left in the middle).
- Use the **system date and time** for new labels — read them, don't estimate.
- **Every pause ends with the briefing** (step 5) — state, the user's offline tasks, and the
  agent's next-session tasks grouped by what they block, each carrying its environment mark.
  It goes in the chat, in the log, *and* in a dated write-once file under `archive/`; a briefing
  that exists only in the chat is lost the moment the session ends. **The archive file carries
  the same grouping and the same environment marks as the log** — the archive is what the user
  reads first when choosing where to work next, so a mark missing there is a mark that does not
  exist.
- **Archive files are never edited after they are written.** If something in one turns out to be
  wrong, the correction goes in the *next* archive file and in the log — not over the old text.
  An archive that gets rewritten stops being evidence of what was believed at the time.
- The user's task list is for what **only a human** can do. Anything the agent can execute
  itself does not belong there.

## Relationship to other skills

- `pause-update` **orchestrates** `log-update` (step 1) — it does not replace it. `log-update` remains the detailed `PROJECT_LOG.md` protocol.
- Keep this skill **project-agnostic** so the same file works in any project's `.claude/skills/`.
