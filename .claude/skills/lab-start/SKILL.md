---
name: lab-start
description: Load this lab's context at the start of a session, then say what you understand and wait. Run this FIRST in every new conversation, before acting.
---

# /lab-start: load the lab before you touch it

A new conversation remembers nothing. This loads enough to act safely, and no more.
Budget: a few minutes. If you are reading the whole folder, stop; you have misread this
file.

## Step 1: read, in this order

1. `AGENTS.md`: the rules you work under. All of it.
2. `work/scope.yaml`, if it exists: what you are allowed to touch, and when. If it does
   not exist, you may analyse files in `samples/` and `work/` but act against no system.
3. `work/WORK_LOG.md`, if it exists: the note the previous session left. Read the top
   entry. A fresh lab has none.
4. The task the person gave you, if any, matched to a skill in the table in `AGENTS.md`.

Do not read every file in `rules/` now. Each says at the top when it applies; load it
when you are about to do that thing.

## Step 2: look at the state

List what is in `samples/` and `work/`, one level deep. List the tools you can use right
now, including any MCP tools. If `work/scope.yaml` exists, compare them with its
`tools_allowed`: a connected tool that the scope does not allow is reported, not used.

## Step 3: say what you understand, then stop

In under 150 words: which folder you are in; whether `work/scope.yaml` exists and what it
allows; what the last session left unfinished; which skill fits the task; what you think
the next step is. Then wait. Do not start the work.

## Related

`/lab-save` when you finish. Load the matching skill when you begin the task.
