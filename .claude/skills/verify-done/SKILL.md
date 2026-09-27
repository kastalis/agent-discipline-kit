---
name: verify-done
description: Before reporting any task as done, and at every pause, check that the state of the project matches what the documents claim — named files exist and are not empty, counts and rows are where the log says, numbers are physically possible. Use before saying "done", before a pause, after any tool reports success, and whenever the owner asks "is it really finished?".
---

# verify-done

## Purpose

In one project, three tools reported success over work they had not done:

1. A renderer printed a tidy log, exact timings per frame and its output folder. The folder had
   been created by the launcher and was **empty** — no frame had been written anywhere on disk.
2. An analytics script wrote **0 views next to 1:20 of average watch time** — an impossible pair.
   It had read a column that the export didn't contain and silently wrote zero.
3. The same script left one post out of its reports for **four days** without a message. It reported
   that it had processed the first post, and stopped.

None of them failed. Each ended with "done". So this skill does not ask whether the tools work.
It asks **whether the state of the project matches what the documents say**.

## The rule

> A tool that ends without an error has only proven that it ended. Show the thing, not the
> message about the thing.

## When the agent reports a task as done

For each claim in the report, give the evidence next to it:

| Claim | Evidence to show |
|---|---|
| "The file was written" | the path, its size, and that it is **not empty** (for images or video: how many) |
| "N items were processed" | the count **from the output**, not from the tool's message — and which items, by name |
| "The row was added to the log / table" | the row itself, quoted, with the section it is in |
| "The measurement is X" | the source of X, and a sanity check against a number it must agree with |
| "Nothing changed / nothing to do" | where you looked; an empty result is a claim about that place only |

If evidence can't be produced, the report says **"not verified"** — never "done".

## Sanity checks that catch most silent failures

- **Impossible pairs.** Zero views with non-zero watch time; a duration longer than the file;
  more items out than came in. If two numbers can't both be true, stop.
- **Missing members.** If the document names five items, the output must name five. Count them.
- **Empty but present.** A folder or file that exists but is empty is a failure, not a success.
- **Full but unfinished.** A file can be long, end cleanly and still hold a placeholder in the
  middle — a `__` marker, a `TODO`, a template line nobody filled in. Search the whole file for
  them; checking only its end won't find one.
- **Checked what, exactly?** When you report a check, name what it looked at ("each file ends
  cleanly"), not what you hope it proved ("the files are fine"). The owner can then see what
  the check could not have caught. The same goes for any claim that something is **saved,
  backed up or protected**: say where it is, who can reach it, and how long it lasts. A backup
  only the agent can open, and only until the session ends, is not a backup for the owner.
- **Written where you think.** Resolve paths once, to an absolute path, before use; after the
  run, look in that exact place.
- **Too-strict guards.** A check that makes everything fail isn't measuring — it's masking. Before
  trusting a green result, plant one deliberate error and see that the check catches it.

## At every pause

Run through the project's own list of claims (the log's `CURRENT STATE` and the documents it
names) and check the ones that can be checked cheaply:

1. Every file the log names exists and is not empty.
2. Every number the log states for today can be traced to where it came from.
3. Size limits the project set for itself (e.g. "current state ≤ 40 lines") still hold.
4. Nothing the log calls "done" is still listed as a task elsewhere.
5. No file edited this session still holds a placeholder (see "Full but unfinished" above).
6. A claim the session found wrong is gone from every file that carried it — search for it.

Report the result as a count: "12 checks, 12 passed" or "12 checks, 1 failed: …". A result
that is a number can be compared with the next pause; an impression can't.

## If the project has code

It pays to turn the pause checks into one small script whose exit code is 0 only if **everything**
passed. In our project the first run of such a script found three real faults — all three had
been reporting success. The checklist in `CHECKLIST.md` is the version for people who don't read
code.
