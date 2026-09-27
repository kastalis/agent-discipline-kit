# Getting started — without git or a terminal

This takes about five minutes. You need an AI agent that can work in a folder on your computer
(for example Claude Code or Cowork in the Claude desktop app, or another vendor's agent), and a
folder for your project — an empty one is fine.

## 1. Let the agent install it — one message

Open your project folder in the agent and paste this. If the app lets you pick both a project
and a working folder when you start, keep **both** selected — without the folder the agent sees
none of your files.

> Install the kit from https://github.com/kastalis/agent-discipline-kit into this folder: copy `AGENTS.md`, `CLAUDE.md`,
> `CHECKLIST.md` and the whole `.claude` folder. If the folder already has an `AGENTS.md` or
> `CLAUDE.md`, don't overwrite it — show me both and merge them with my approval. Then follow
> `AGENTS.md`.

That's all. The agent copies the files, notices that the project is not set up yet, and starts
asking you questions.

_(We have tested this with Claude only. If your agent can't download from the web, use step 1b.)_

### 1b. If the agent can't download — copy the files yourself

1. On this repository's page, click **Code → Download ZIP** and unzip it anywhere.
2. The folder `.claude` is **hidden** on most systems, and it's the easiest one to miss. Show it
   first — on Windows: File Explorer → **View → Show → Hidden items**; on macOS: press
   **Cmd + Shift + .** in Finder.
3. Copy `AGENTS.md`, `CLAUDE.md`, `CHECKLIST.md` and the whole `.claude` folder into your project
   folder.
4. Open the folder in your agent and write anything — "hello" is enough.

## 2. First session — answer the questions

The agent asks a few questions, one at a time: what the project is called and what it is for, who
you are, which language you want to talk in, and what it must never do without asking. It writes
the answers into `AGENTS.md`, starts `PROJECT_LOG.md`, and tells you the three habits that matter.

If you'd rather start working first, tell it to skip the setup — it will ask again next time.

## 3. Every session after that

- **Start:** the agent reads the log and asks you three questions (what you ran, what you did by
  hand, what you saw and didn't report). Answer them, even if the answer is "nothing".
- **During:** when it asks for something concrete, give it — that's the point.
- **End:** say **"pause"**, in any language. The agent updates the log, writes a snapshot in
  `archive/`, and gives you the plan for next time. It will ask how many hours you spent — tell it.
- **Once a week:** go through `CHECKLIST.md` yourself. Ask the agent for a copy in your language.

## Upgrading to a new version

Your `AGENTS.md` holds your project, so it is never simply overwritten. Copy the new skills,
`CHECKLIST.md` and `docs/` over the old ones, and let the agent merge `AGENTS.md` with your
approval — the message to paste is at the top of [`CHANGELOG.md`](../CHANGELOG.md).

## Which file is for which agent?

`AGENTS.md` holds all the rules; many agents read a file with that name on their own. `CLAUDE.md`
contains only a short pointer to `AGENTS.md`, because Claude looks for that name. You don't need to
change or rename either one — the kit works the same whichever agent you use. If your agent
doesn't load the skills from `.claude/skills/` by itself, `AGENTS.md` tells it where to read them.

## Using git later (optional)

Nothing here needs git. If you start using it, the log and the snapshots in `archive/` become
even more useful: you can see exactly what changed between two pauses.
