# Data handling and secrets

> **Load this when:** you are about to send data to the model, save a file, commit, or
> touch anything that authenticates.

## What leaves the laptop

Everything the agent sends to the model goes to the model provider: the prompt, the
files you let it read, the tool outputs. With your Cybersteps Claude account it goes to
Anthropic; with another provider, to that provider and the company that hosts the model;
with a local model through Ollama it stays on the laptop.

**So:** work only with synthetic data (`samples/`) or data from the lab. Never real
logs from an employer, real mailboxes or real personal data. If you need to analyse
something sensitive at work, that is a decision for your employer, and a local model is
one of the options.

## Secrets

- No key, token, password or connection string goes into a note, a file in `work/`, a
  report or a prompt. If you get a key from your instructor, it lives only in the file
  the instructions name (such as `openrouter.json`, which git ignores).
- A secret that was pasted into a chat or a file is burned: tell the instructor, who
  replaces it. Deleting the line does not undo it.

## Before you share your work

`work/` stays on your laptop: git ignores it, so nothing in it is ever pushed. If you
copy a report somewhere to share it, for example into a portfolio, check it first: lab
addresses, accounts or screenshots with sessions do not belong there.
