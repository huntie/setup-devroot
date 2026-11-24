# devroot ("huntiestack")

[![skills.sh](https://skills.sh/b/huntie/setup-devroot)](https://skills.sh/huntie/setup-devroot)

**One workspace for you and your agents.**

<img width="250" src="https://github.com/user-attachments/assets/2fa0e819-8d8a-435c-bad9-f2f147984a1f" />

[Rebuilding Meta's Dev Stack Out of Meta | React Native Live](https://www.youtube.com/live/zazUsINu9AQ?si=v33vCNORWM08H9D_&t=613)

## What is devroot?

devroot is an opinionated dev setup built around one repo: a personal, private workspace where you and your agents do all your development work. It can replace your `~/Development` folder, or live anywhere you want.

`devroot` gives your agent a **superpowered frame** to work in:
- A folder structure, scripts, and skills empowering it to checkout your projects **and work across them**.
- Context stored in a private shared `vault` providing **self-improving personal knowledge**.
- An refined commit workflow for high quality PRs and **Meta-like source control** (optional).
- All of this, portable across sessions and machines, meaning you can trivially set up bots that **behave like you**.

This repo is a public starter kit for this setup, mirrored from my personal devroot.

## Get started

Ask your agent to set up your devroot repo:

    Set up my devroot.
    Follow https://raw.githubusercontent.com/huntie/setup-devroot/main/INSTALL.md

Your agent asks where the devroot should go, then fetches `devroot-template-latest/`, installs [skills](https://github.com/huntie/skills), writes the initial commit, and runs the minimal workspace setup script.

## Use directly or take inspiration

The full devroot layout:

```sh
devroot/
  # Optional: <user>/dotfiles — system dotfiles 
  dotfiles/

  # Working directories
  projects/        # Project checkouts (gitignored)
  scratch/         # Throwaway checkouts (gitignored)

  # Repo scripts (`dev <script>`)
  scripts/

  # Skills (installed to .[agents|claude]|/skills/)
  skills/          # Your skills
  skills-lock.json # Third party skills

  # Optional: <user>/vault repo — agent plans + knowledge base (private)
  vault/
    memory/              # Workspace + per-repo knowledge (/reflect)
    plans/               # Numbered plans (/plan)
    index.md             # Index of all content
    USER.md              # Optional: Who you are and your goals
    COMMITTING_POLICY.md # How sessions commit here
```
