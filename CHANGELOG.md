# Changelog

What changed in the template between versions, so a project that already filled in its
`AGENTS.md` can upgrade without comparing files line by line.

## How to upgrade a filled-in project

Copy the new skills (the whole `.claude/skills/` folder), `CHECKLIST.md` and `docs/` over the old
ones. **Don't copy `AGENTS.md`** — it holds your project. Instead, put the new one in a separate
folder **outside your project folder**, and tell your agent:

> Read `CHANGELOG.md` from version <your version> on. Merge those changes from the new
> `AGENTS.md` into mine, and show me each change before you make it. Keep everything I filled
> in: `PROJECT`, the language line, my rows in `RESOURCES` and my own constraints.

Your version is on the third line of your `AGENTS.md`.

Why outside: an agent that reads subfolders will find the new kit's `.claude/skills/` too, and load
a second set of skills next to yours. During an upgrade the two sets differ by definition, so the
agent can run the wrong version. If you must keep the new kit inside the project, delete its
`.claude/` folder once the upgrade is done.

You don't need to keep the old skills for comparison: this file lists what changed in them, and
the agent can work from it.

---

## 1.5.1 — 2026-09-27

From the fourth test session in a real project (an upgrade from 1.4 to 1.5, moving from one agent
environment to another). Two fixes, both for errors that repeat or do damage:

- `pause-update`: the search for leftovers also looks for a name in curly braces (`{date}`) — a
  line of code that wrote its template instead of its value. It left a file name in the log that
  pointed to no file, and the list of markers did not catch it.
- This file, **How to upgrade:** the new kit goes outside the project folder. Kept inside, its
  skills were loaded a second time, next to the project's own.

---

## 1.5 — 2026-09-26

From the third test session in a real project (an upgrade from 1.3 to 1.4 and a pause).

- `log-update` and `pause-update`: the time label is read **in the same step that writes it**; a
  time remembered from earlier in the conversation counts as an estimate. The rule "from the
  system clock" alone was broken in two sessions in a row.
- `log-update` and `pause-update`: every task keeps **one number** until it is done; nothing is
  renumbered, and group letters mark priority, not identity. `ACTIVE TASKS` gets a `#` column.
  `AGENTS.md` → **Conventions** carries both rules in one line each.
- `pause-update`: a wrong memory the agent can't correct becomes a task on the owner's list.
- `verify-done`: a claim that something is saved or backed up says where, who can reach it and
  for how long.
- `AGENTS.md` → **Behaviour:** without version control, a copy made before a change goes into
  the project folder (`archive/before-change-…/`), not a place only the agent can reach.
- `CHECKLIST.md`: one pause item (task numbers mean the same task as last time).
- This file: the old skills don't need to be kept for an upgrade.

---

## 1.4 — 2026-09-25

From the second test session in a real project (an upgrade from 1.2 and a first pause on 1.3).

- `AGENTS.md` → **Session start:** the three questions close the agent's **first reply**, whatever
  the owner opened with; nothing from memory is named until the folder has been looked at. The
  questions had been skipped in two sessions in a row.
- `AGENTS.md` → **Say where knowledge comes from:** a remembered fact written into a file is still
  a remembered fact; a claim found wrong is corrected everywhere it is written, the same session.
  The first interview writes into `PROJECT` only what the owner said or a file showed.
- `AGENTS.md` → **Pause** and **Conventions:** if the agent can't see the model picker, it asks the
  owner for the model and level names once and writes them into `CONVENTIONS`. Dates **and
  times** come from the system clock.
- `pause-update`: files are read back **whole** and searched for leftover placeholders, not only
  checked at the end; memory proven wrong is corrected; the chat briefing is written from
  `NEXT SESSION`, not separately; a closing message that must go into a file is written there
  before it is sent.
- `verify-done`: two sanity checks (placeholders; say what a check looked at) and two pause checks.
- `log-update`: time labels from the system clock.
- `ask-dont-guess`: the three questions tied to the first reply.
- `CHECKLIST.md`: two pause items. New file: this changelog.

## 1.3 — 2026-09-24

- `pause-update`: every agent task in the plan carries a model and thinking level, named as the
  owner's tool names them; memory is kept wherever the agent keeps it.
- `log-update`: `NEXT SESSION` is part of the log template; `Updated by` names environment and
  model.
- `AGENTS.md`: the three questions are skipped in the setup session; new rule "Say where knowledge
  comes from"; the pause doesn't ask for hours the owner already gave.
- `docs/getting-started.md`: keep both the project and the folder selected when starting.

## 1.2 — 2026-09-24

- The template became `AGENTS.md`, independent of the agent's vendor; `CLAUDE.md` only points to
  it.
- First-session interview: an empty `PROJECT` makes the agent set the project up before working.
- Install in one message.

## 1.1 — 2026-09-22

First published form: `CLAUDE.md` template with `why` lines, four skills, `CHECKLIST.md`,
getting-started guide without git.
