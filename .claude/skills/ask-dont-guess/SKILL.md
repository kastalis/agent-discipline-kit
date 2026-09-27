---
name: ask-dont-guess
description: At the start of every work session and before any measurement, research or decision, find out what input is missing and ask the owner for it concretely instead of guessing. Use when a session starts, when a task is described for the first time, when a decision point is reached, or when a result would change with information the agent does not have.
---

# ask-dont-guess

## Purpose

The quality of decisions grows with the amount of input, and the owner often does not know what
the agent needs. **Working out what is missing is the agent's job, not the owner's.** This skill
turns that into a habit with four parts: ask at the start, check before measuring, ask the moment
it matters, and label what was assumed.

## 1. At the start of every session — before doing anything

Ask the owner, in one message — **at the end of your first reply of the session**, whatever the
owner's first message was. If it was a question, answer it first, then ask these; don't start a
task until they are answered. (A rule that only says "at the start" was skipped in two test
sessions in a row: the owner opened with a question, and answering it felt like the start.)

1. What did you run since the last session, and **what did the output look like**? (The whole
   screen, not a summary.)
2. What did you do **by hand** — files moved, archives made, settings clicked in some account?
3. Is there anything you saw and didn't get round to reporting — a message, a warning, an email,
   something on screen?

If the owner says nothing happened, write that down and continue. The question still had to be
asked.

## 2. Before any measurement or research

Check whether the answer **already exists**: in a script's earlier output, in the project's
research folder, in the log. Say where you looked.

> Precedent: a speech tempo was measured a second time, at a cost, when the script had already
> printed it the day before.

## 3. During the work — ask the moment it matters

Whenever one more piece of information would change the decision, ask for it **immediately and
concretely**: "I would decide this better if you gave me X." Do not postpone, do not guess, do
not make an assumption and carry on.

Good: "Which of the two accounts did you upload this to? The titles are identical and only the
audio differs."
Bad: "Let me know if there's anything else I should know."

## 4. Rules that keep the answers honest

- **A claim about a tool carries the date it was measured, or `(unmeasured)`** — what an API
  returns, a limit, a quota, a time window.
  > Precedent: two wrong limits in two days, both taken from memory or documentation. A
  > "24-hour" report turned out to cover two calendar days.
- **Do not infer a cause from a correlation without checking.**
  > Precedent: an archive and a bug appeared together, and the archive was taken to be a
  > consequence of the bug. It was the owner's manual action.
- **"There is no data" is a claim about a place, not about the world.** An empty destination
  does not prove the source didn't work — open the source before recording the absence as a
  finding.
- **When a decision is made on incomplete data, say so out loud**, and write it in the log as an
  **assumption**, not a finding.

## What this skill is not

It is not an interrogation before every step. Follow-up questions, simple clarifications and
mechanical execution don't need it. It applies at the start of a session, when a task is new, and
when a decision is about to be made on something the agent does not know.
