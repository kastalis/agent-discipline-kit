# Is it really done? — a five-minute check for the owner

You don't need to read code for any of this. Open the project folder and the log, and ask the
agent to show you each item. Tick it only when you have **seen** it, not when you were told.

## After any task the agent calls "done"

- [ ] I saw the file (or files) it produced — they are where it says, and they are **not empty**.
      For pictures or video: the count matches what was asked for.
- [ ] If it processed a list (posts, customers, invoices), the output names **every** item on
      the list, not just "done, 5 items".
- [ ] The numbers can both be true. (Zero views with a minute of watch time can't. Twelve
      replies to ten emails can't.)
- [ ] It told me what it did **not** check, or that everything was checked.

## At every pause (end of a work session)

- [ ] `PROJECT_LOG.md` has today's date at the top, and `CURRENT STATE` describes today — not
      last week.
- [ ] Every task the log calls done is really done (spot-check one: open the file, and scroll
      through it — not just to the end).
- [ ] If the agent found out today that something it believed was wrong, the wrong version is
      gone from `AGENTS.md` too, not only mentioned in the log.
- [ ] The plan for the next session says who does what: me, or the agent, and where.
- [ ] A task number in the plan means the same task it meant in the last snapshot in `archive/`.
- [ ] A new dated snapshot is in `archive/`, and it opens with what was done, what wasn't, and
      what happened that wasn't planned.
- [ ] My hours for this session are written down.
- [ ] Anything that cost money today is written down with the amount.
- [ ] If anything ran outside my computer today (a cloud session, a scheduled job), the agent
      told me **before** it started where it is billed and what stops it at the limit.

## Before anything goes public

- [ ] Nothing in it comes from my employer or a client — names, numbers, screenshots, file paths.
- [ ] No private data: phone numbers, personal email, home address, passwords, keys.
- [ ] Anything taken from someone else names its author and its license.
- [ ] I read it myself, start to finish.

## When something fails a check

Don't fix it yourself. Tell the agent what you saw, and ask it to find **why the tool reported
success anyway**. The fault you found matters less than the silence that hid it — fix the silence
and the next fault will announce itself.
