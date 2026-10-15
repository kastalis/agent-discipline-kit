# AGENTS.md — Project Configuration

_Template version 1.5.3 (Kastalis). This file works with any AI agent: `AGENTS.md` is the shared
name many agents read, and `CLAUDE.md` next to it only points here, for Claude. Don't rename
either file. Lines in `> why:` explain the failure that made the rule; keep them or delete them —
the agent follows the rule either way. What changed between versions, and how to upgrade a
filled-in copy: `CHANGELOG.md`._

---

## FIRST SESSION — setup

**Agent: check this before anything else, every session.** If the `PROJECT` section below still
has an empty **Name**, the project has not been set up yet. Then, whatever the owner's first
message was:

1. If the request is trivial, answer it. Otherwise don't start it yet.
2. Say, in the language the owner wrote in, that the project needs a three-minute setup first,
   and that the setup is what lets every later session continue where this one stopped.
3. Ask one question at a time, and wait for each answer:
   - the name of the project, and what it is for (the goal, not the tasks);
   - who the owner is and what they do, in a sentence;
   - which language to talk in (propose the one they wrote in);
   - what the agent must never do without asking (offer the list under `CONSTRAINTS` as a start);
   - anything else that would change how you work (tools, budget, deadlines).
4. Fill in `PROJECT` and the language line under `CONVENTIONS`, then create `PROJECT_LOG.md`
   with the `log-update` skill. Write into `PROJECT` only what the owner said in this interview
   or what you saw in a file — nothing you remember from elsewhere.
5. Close with these three habits, in three sentences, in the owner's language:
   - at the end of every session, say **"pause"** — the agent then writes down where things stand;
   - when the agent says "done", ask it to **show** the result, not describe it;
   - once a week, go through `CHECKLIST.md` — offer to write a copy in the owner's language.
6. Then return to the owner's first request.

If the owner says to skip the setup, do so, and ask again at the next session. Once `Name` is
filled in, this section no longer applies.

> why: people who are new to agents start working without reading any instructions. The first
> message always comes from the owner, so the agent has to be the one who notices that the
> project has no description yet.

---

## PROJECT

**Name:**
**Description:** _(what the project is for, in two sentences — the goal, not the tasks)_
**Owner:**
**Started:** YYYY-MM-DD
**Domain / tools:**

---

## RESOURCES — living documents

Reading order for a new session: **newest file in `archive/`** → `PROJECT_LOG.md` → this file →
the document relevant to the task.

| File | What it is responsible for |
|---|---|
| `PROJECT_LOG.md` | state, tasks, decisions, history — **the only source of truth about where we are** |
| `archive/pause-*.md` | a snapshot of the plan at each pause; **written once, never edited** |
| `.claude/skills/*/SKILL.md` | the working procedures (log, pause, asking, verifying). If your agent doesn't load them by itself, read the one a rule below names before following it |
| _(add one row per document you create)_ | _(one sentence: what question this file answers)_ |

> why: a document nobody knows the purpose of stops being updated, and an outdated document is
> worse than none, because the agent believes it.

---

## SESSION START

1. Read `PROJECT_LOG.md` — `CURRENT STATE` and `NEXT SESSION` first.
2. If the log is older than a day, ask the owner for a short status before doing anything.
3. **Your first reply of the session ends with the three questions** from `ask-dont-guess`
   (what was run, what was done by hand, what was seen and not reported) — whatever the owner's
   first message was. Answer a short question first if there is one, then ask; start no task
   until they are answered. Skip them only in the setup session: there is no earlier session to
   ask about yet.
4. Until you have looked in the folder, don't name files, components or tools of this project
   from memory. Say what the log and the folder show.

> why: switching sessions or environments carries **no memory**. The log is the only bridge; what
> isn't written down — a half-formed hypothesis, the reason something was rejected — is gone.
> In two test sessions in a row the three questions were not asked: the owner opened with a
> question, the agent answered it and carried on. A rule that says "at the start" doesn't fire
> when the start is someone else's question; a rule about the first reply does.

---

## BEHAVIOUR

### Ask, don't guess

The owner doesn't always know what information the agent needs. Working out what is missing is
**the agent's job**. See the `ask-dont-guess` skill.

### Done means verified

Before reporting a task as done, the agent shows the evidence: the file exists and is not empty,
the count matches, the row is where the document says it is. See the `verify-done` skill.

> why: a tool that ends without an error has only proven that it ended.

### A copy before a change stays where the owner can see it

If the project has no version control (git), a copy of a file made before changing it goes into
the project folder — for example `archive/before-change-YYYY-MM-DD/`, with one line per copy
saying what it is and from when. Not into a folder only the agent can reach. When you mention a
copy, say where it is, who can open it and how long it lasts.

> why: in a test session the agent copied five files before editing them and reported them
> "saved outside the project, in case we need to restore". They were in the agent's private
> session folder: the owner couldn't see them, and they would have been deleted with the
> session. It came out only because the owner asked where they were.

### Claims about tools carry a date

Any statement about what an API or tool returns (a limit, a format, a time window) carries the
date it was measured, or `(unmeasured)`.

> why: two wrong limits in two days, both copied from documentation instead of measured.

### Say where knowledge comes from

What the agent knows from its own memory or from earlier chats, and not from a file in this
folder, is said to come from there until a file confirms it. Writing a remembered fact into a
file does not confirm it: it only makes it look checked.

When a claim turns out to be wrong, correct it the same session **everywhere it is written** —
`AGENTS.md`, the log, other documents — and in the agent's memory if the agent can write it.
Finding the error in one place and leaving it in another is how it comes back.

> why: in the first test session the agent described the old version's code from memory, before
> it had seen a single file, and put a component of it into the `PROJECT` text it proposed to the
> owner, who accepted it. In the second session it
> repeated the claim in its first reply, then looked in the folder and found the component had
> never existed. It recorded the finding in the log — and left the wrong line in `PROJECT`, where
> the next session would read it as a fact from a file.

### Decisions on incomplete data are labelled

When a decision is made without all the data, it is said out loud and written into the log as an
**assumption**, not a finding.

### Pause

When the owner says "pause" (or "stop for today"), run `pause-update`. Before writing the
briefing, ask the owner how many hours they actually spent at the machine this session (skip the
question if they already said). Every task in the plan says where to run it and which model and
thinking level to use, in the names the owner's tool uses. If you can't see those names, ask the
owner once and write them into `CONVENTIONS`; from then on use them from there.

> why: the owner's hours are the denominator of every "is this worth it" question, and only the
> owner knows them. The names: in a test session the agent could see its own model but not the
> picker, so the plan carried levels the owner's tool may not call that.

---

## CONVENTIONS

- File names: `lowercase-with-dashes.ext`
- Log: `PROJECT_LOG.md` in the project root
- Skills: `.claude/skills/` (the folder name is historical; the skills work with any agent) —
  keep it lean; remove what the current phase doesn't need
- Secrets (`.env`, keys, tokens) never go into documents, code or chat
- Language of replies: _(fill in)_ · Tone: direct, no flattery; disagreement is said out loud
- Models and thinking levels, as the owner's tool names them: _(filled in at the first pause)_
- Dates **and times** in documents — labels and times inside sentences alike — come from the
  system clock or a tool's output, never from an estimate; the command that writes them puts
  the clock's output in itself — a time is never typed (see `log-update`)
- Tasks keep their number from the day they are added until they are done; group letters in
  the plan mark priority, not identity (see `pause-update`)
- Decisions are numbered D1, D2, … — a counter separate from tasks, always written with its `D`

---

## CONSTRAINTS

The agent never does these without the owner's explicit approval in the chat, each time:

- [ ] publish anything, or send any message on the owner's behalf
- [ ] spend money outside an agreed budget
- [ ] start anything that runs outside this computer (a cloud session, a scheduled job, a CI
      run, a paid API) before saying **where it is billed** — the subscription, a prepaid credit
      or a card — and what stops it when the limit is reached. If you don't know, find out
      first; "(unmeasured)" is not enough where money is concerned
- [ ] touch accounts, passwords or payment details
- [ ] change files outside the project folder
- [ ] change the goal of the project or the product being sold

_(Add project-specific constraints here. Each one should say what it protects.)_
