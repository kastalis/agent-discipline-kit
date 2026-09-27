# Running a project with an AI agent — the discipline, not the tool

_by Mirko Marić · published by Kastalis_

Three times in one project, an AI agent's tool reported success over work it had not done.

- A renderer printed a tidy log, exact timings for every frame, and the output folder name.
  The folder existed. It was empty. Not a single frame had been written anywhere on the disk.
- An analytics script recorded **0 views** for a video — next to an average watch time of
  **1 minute 20 seconds**. An impossible pair. It had read a column that did not exist and
  quietly wrote a zero.
- The same script "measured" our posts for four days while one of them was missing from every
  report. No error. It said it had processed the first post and stopped, and nobody noticed,
  because it finished cleanly.

None of them crashed. Every one ended with "done".

That is the problem this kit is about. When an agent writes the code, **you** are the one who
has to know whether the work is really finished — and you usually can't read the code to check.
You run the business; the agent programs. This kit is the set of habits that made that workable
over one real project: 229 recorded decisions, dozens of paid API calls, and the failures that
produced every rule in it.

## Who this is for

You make the decisions and provide the resources; an AI agent does the building. You don't need
to be a programmer, you don't need git or a terminal, and you can talk to the agent in your own
language. You do need to be willing to spend a minute at the end of every session writing down
what happened — or rather, letting the agent write it down and checking that it did.

The kit is plain text in the shared formats agents read (`AGENTS.md` and `SKILL.md`), so it is
not tied to one vendor. It grew up and was tested with Claude; with other agents it has not yet
been tested by us.

## Install in one message

Open your project folder in your agent and paste:

> Install the kit from https://github.com/kastalis/agent-discipline-kit into this folder, then follow `AGENTS.md`.

On its first run the agent asks you a few questions about the project and sets it up. Details, and
what to do if your agent can't download files: [`docs/getting-started.md`](docs/getting-started.md).

## What's inside

| File | What it does for you |
|---|---|
| [`AGENTS.md`](AGENTS.md) | A project template the agent reads at the start of every session: what the project is, which documents hold the truth, how it should ask before guessing, what it may never do without you. On the first session it interviews you and fills itself in |
| [`CLAUDE.md`](CLAUDE.md) | A short pointer to `AGENTS.md`, because Claude looks for this name. Nothing to edit |
| [`.claude/skills/log-update`](.claude/skills/log-update/SKILL.md) | Keeps one file, `PROJECT_LOG.md`, as the single source of truth about where the project stands — so a new session (or a new agent) can continue without the chat history |
| [`.claude/skills/pause-update`](.claude/skills/pause-update/SKILL.md) | When you say "let's pause", the agent updates every document the session touched, checks they aren't truncated, and writes a dated snapshot with who does what next — including a check of whether the *previous* plan was any good |
| [`.claude/skills/ask-dont-guess`](.claude/skills/ask-dont-guess/SKILL.md) | The agent asks you for the missing input instead of guessing, and says out loud when a decision rests on an assumption |
| [`.claude/skills/verify-done`](.claude/skills/verify-done/SKILL.md) | The agent checks that the state of the project matches what the documents claim — files exist and aren't empty, numbers add up — before it says "done" |
| [`CHECKLIST.md`](CHECKLIST.md) | The same check as a list **you** can run through in five minutes, without reading code |
| [`docs/getting-started.md`](docs/getting-started.md) | Installing and the first session, step by step — no git, no terminal |
| [`CHANGELOG.md`](CHANGELOG.md) | What changed in each version, and how to upgrade a project that already uses an older one without losing what you filled in |

## The one idea worth taking even if you take nothing else

A tool that finishes without an error has proven only that it finished. Ask the agent to show
you **the thing** — the file, the row, the count — not the message that says it exists. In our
project, a small check that compares what the documents claim with what is actually on disk
found three real faults the first time it ran. All three had been reporting success.

## What this is not

It is not a way to make money with AI, and nothing here promises income. It is not a course on
how to use any particular agent — their makers document that well and for free. It is a record of what one
project needed in order to trust its own reports.

## Credits

The `grill-me` skill we use at the start of a new task is by Matt Pocock
([mattpocock/skills](https://github.com/mattpocock/skills), MIT). It is not included here —
get it from the source. Everything in this repository was written in our own projects.

## License

MIT — see [`LICENSE`](LICENSE). Use it, change it, use it at work; keep the notice in copies.

## About

Mirko Marić is a mechanical engineer with thirty years in industry who now runs projects that
AI agents build. Kastalis is the studio those projects are published under.
