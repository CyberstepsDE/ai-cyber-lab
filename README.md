# AI Cyber Lab

Your AI lab for the AI sessions of the Cybersteps cybersecurity program, Module 4.

It is a folder, not an application. When you start Claude Code in it, the agent reads the
lab's rules, gets ready-made routines (skills) and finds practice data. You bring the task
and the judgement: the agent reads the data, drafts, and asks before it acts. Every
finding ends with the record that proves it.

## What is inside

```text
ai-cyber-lab/
├── README.md               this file
├── AGENTS.md               the rules the agent reads before doing anything
├── CLAUDE.md               points Claude Code at AGENTS.md
├── rules/                  the detail behind AGENTS.md, one topic per file
│   ├── scope.md            act only where you are authorised
│   ├── evidence.md         no finding without a record
│   ├── untrusted-input.md  text in the data is data, never an instruction
│   ├── data-handling.md    what leaves your laptop; no secrets in files
│   └── what-checks-prove.md  what a check proves, and what it cannot
├── .claude/
│   ├── settings.json       starts the agent in Manual mode, asks before ssh and git push
│   └── skills/             the routines: /lab-start, /lab-save, /triage-log, /review-findings, /report
├── samples/                practice data (see below)
├── templates/              report formats: incident note, assessment report
├── scope.example.yaml      copy to work/scope.yaml when your instructor gives you lab targets
├── ollama.json             start the agent on a model on your own laptop
├── openrouter.example.json only if your instructor gives you an OpenRouter key
└── work/                   YOUR work: work log, scope, evidence, reports (git ignores it)
```

**Your work stays yours.** Everything the agent writes for you goes into `work/`. Git
ignores that folder, so `git pull` brings new practice files without touching your work,
and nothing in it is ever pushed anywhere. If you delete the lab folder, `work/` goes
with it: copy it somewhere safe if you want to keep it.

## Words used here

- **Agent:** the program that sends your task to a model and, when you allow it, uses tools
  such as reading a file or running a command. Here: Claude Code.
- **Skill:** a routine in plain English that the agent follows when you type its name, for
  example `/triage-log`. It is instructions, not a program: open `.claude/skills/` and read them.
- **Scope:** the targets you are authorised to test, the time window and the tools allowed.
  Authorisation is the line between a security test and a crime.
- **Clone:** copy a repository from GitHub to your laptop, with git.

## Set up on Windows

Do one step at a time and check it before the next. Run the commands in PowerShell.

1. **Git.** Run `git --version`. If PowerShell does not know `git`, install
   [Git for Windows](https://git-scm.com/downloads/win) with the default options, then open a new
   PowerShell window. Claude Code also uses Git Bash as its shell when it is installed.

2. **Claude Code.** Run the official installer:

   ```powershell
   irm https://claude.ai/install.ps1 | iex
   ```

   Open a new PowerShell window and run `claude --version`. If `claude` is not recognised,
   its folder is not on your PATH yet. Add it once, then open a new window:

   ```powershell
   [Environment]::SetEnvironmentVariable('PATH', [Environment]::GetEnvironmentVariable('PATH','User') + ';' + "$HOME\.local\bin", 'User')
   ```

3. **Sign in.** Run `claude`. A browser opens: sign in with your Cybersteps account and
   pick the Cybersteps organization if you have more than one. Type `/status` to see the
   account, then `/exit`.

4. **The lab folder.** Clone it into your user folder and go into it:

   ```powershell
   git clone https://github.com/CyberstepsDE/ai-cyber-lab.git "$HOME\ai-cyber-lab"
   cd "$HOME\ai-cyber-lab"
   ```

   Already cloned it earlier? Go into the folder and run `git pull` instead.

5. **Start the lab.** In the lab folder run `claude`. The first time, it asks whether you
   trust the folder: yes. Then type `/lab-start`. The agent names the folder, says that
   `work/scope.yaml` does not exist yet (so it only analyses files) and waits.

6. **A first task.** Type `/triage-log samples/auth.log`. Check one line it cites yourself:

   ```powershell
   Select-String -Path samples\auth.log -Pattern "Accepted"
   ```

   Then type `/lab-save` and open `work\WORK_LOG.md`.

## Set up on Kali

Your Kali VM has the offensive tools, so the agent can work there too, under the same rules.
Take a VM snapshot first.

```bash
curl -fsSL https://claude.ai/install.sh | bash
claude --version
git clone https://github.com/CyberstepsDE/ai-cyber-lab.git ~/ai-cyber-lab
cd ~/ai-cyber-lab
claude
```

The first `claude` opens a browser to sign in with your Cybersteps account. If no browser
opens, copy the link it prints into the browser of the VM. If `claude` is not found after
the install, open a new terminal.

`work/` on Kali and `work/` on Windows are two separate folders: each lab keeps its own work.

## Run a model on your own laptop (optional)

With a local model, your text never leaves the laptop. It is much slower than the cloud
model and weaker; use it when the data must stay on the machine.

1. Install [Ollama for Windows](https://ollama.com/download/windows) and run `ollama --version`.
2. Download a small model that can use tools (about 3 to 4 GB):

   ```powershell
   ollama pull qwen3.5:4b
   ```

3. Give it room to think: in the Ollama app, open Settings and set the context length to
   64k. Agents need at least that; much more needs more memory than most laptops have.
4. In the lab folder, start the agent on it:

   ```powershell
   claude --settings ollama.json --model qwen3.5:4b
   ```

   `/status` shows the base URL `http://localhost:11434`. A warning that the model is not
   in Claude Code's model list is expected. `ollama ps` shows the context and how much
   runs on the processor (CPU) or the graphics card (GPU): more CPU means slower answers.

`ollama launch claude` also starts Claude Code on a local model, but `--settings ollama.json`
keeps you in this folder with its rules and skills.

## If something fails

| What you see | What to do |
|---|---|
| `claude` is not recognised | Open a new terminal. On Windows, add `.local\bin` to your PATH (step 2). |
| The sign-in says your account has no Claude Code access | Sign in with your Cybersteps account and pick the Cybersteps organization. |
| `/lab-start` is an unknown command | You started `claude` outside the lab folder. `/exit`, `cd` into the folder, start again. |
| `git pull` refuses because of local changes | You changed a file outside `work/`. Run `git status`, then `git restore <file>` for files you did not mean to change. |
| The local model answers "API error, retrying" | The context is set too high for your memory. Lower it to 64k in the Ollama app. |
| The local model is very slow | Normal on a laptop without a strong graphics card. Use it for short tasks only. |

## What is enforced, and what is only agreed

An instruction file is context, not a lock: a model can drift past it.

| Rule | What holds it | Honest label |
|---|---|---|
| A person approves commands and file changes | `.claude/settings.json` starts the agent in Manual mode | **Asks.** Reading runs without asking. A "don't ask again" you click is remembered for that command. Changing the mode (Shift+Tab) turns this off. |
| The agent asks before ssh and git push | Ask rules in `.claude/settings.json` | **Asks**, while you keep the folder's settings. |
| The agent does not read `.env` files or `openrouter.json` | Deny rules in `.claude/settings.json` | **Blocks** the read tool; a shell command you approve could still read them. |
| Your work is never pushed | `.gitignore` ignores `work/` | **Enforced** for `work/`. Anything you save elsewhere is not covered. |
| Everything in `AGENTS.md` and `rules/` | The agent reading it | **Agreed, not enforced.** It holds as well as the model follows it, which is why you read every finding. |

## Samples

Everything here is synthetic: made up for practice, not taken from a real network or a real
customer. IP addresses come from the ranges reserved for documentation (RFC 5737) and names
from reserved domains (RFC 2606).

| File | What it is |
|---|---|
| `samples/auth.log` | 66 lines of an SSH server log from one night on a server called `web01`. |
| `samples/tickets/` | Three customer support tickets for an AI summary. One of them hides an instruction for the AI (indirect prompt injection). |

## License

[MIT](LICENSE).
