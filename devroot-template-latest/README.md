# devroot

This is your personal development monorepo, created by [setup-devroot](https://github.com/huntie/setup-devroot).

#### Table of contents

- [How to use devroot](#how-to-use-devroot)
- [Layout and conventions](#layout-and-conventions)
- [Workflow](#workflow)⁣
- [User reference](#user-reference)

## How to use devroot

This folder is your **devroot**, a special workspace repo intended to house all of your development work.

It includes opinionated concepts, scripts, and skills that create a capable agent-first dev environment that lets you work continuously across machines.

You will have initialized this repo at `~/devroot/` (or another location of your choosing). "devroot" is the concept name (followed in `AGENTS.md` and skills) regardless of directory name.

#### Getting started

- **You should push this repo** to GitHub now, making that repo private.
- Per machine, run `./scripts/setup` once (documented below).
    - If you have a `dotfiles` repo, this also clones it at `devroot/dotfiles`.
- **All your work now lives here(!)**
    - **Every agent session should start in `devroot/`.** Just ask to clone a repo and do work, and your agent will make it happen.
    - Working repo checkouts (your actual projects) live in `devroot/projects/`. If using an IDE, `devroot.code-workspace` gives you one view across everything.

#### Recommended

- Run the `/setup-vault` skill (one time) to create the `vault` folder linked to your personal `<user>/vault` repo. This is how .

#### This is your repo now(!)

- Consider everything a starter layout, which can now evolve to suit your needs.
- Feel free to drop skills that aren't working for you, install others (`npx skills add`), or add your own scripts. The `vault` should naturally work for your needs over time.

#### Maintaining devroot

- [setup-devroot](https://github.com/huntie/setup-devroot) also includes an `/update-devroot` skill. Run this if you wish to compare and bring in any upstream changes from the latest version.

Happy shipping,\
Alex 👨🏻‍💻\
[Follow me on X](https://x.com/huntie) • [Feedback](https://github.com/huntie/setup-devroot/issues)

## Layout and conventions

#### The main ideas

- **One development monorepo.** I often work across more than one open source repo to accomplish an idea. Agent sessions rooted at `~/devroot` are empowered to work across these. My `dotfiles` and agent skills can be maintained continuously in the same session.
- **Shared config, skills, and context across machines.** One clone carries the entire setup to a Mac, a dev server, or a cloud agent environment. Skills load from the committed agent directories with no install step. `vault` is a live, intentionally updated knowledge base.
- **Stacked version control and targeted FB-isms.** Scripts, skills, and IDE setup smoothing over daily workflows and a purposeful developer loop — for Git or [Sapling](https://sapling-scm.com/).

#### Folder layout

```sh
devroot/
  # Optional core checkouts, ensured by setup
  dotfiles/        # <user>/dotfiles — system/user preferences
  vault/           # <user>/vault — agent plans + knowledge base (private)

  # Working directories
  projects/        # Project checkouts (gitignored)
  scratch/         # Throwaway checkouts (gitignored)

  # Repo scripts (`dev <script>`)
  scripts/

  # Skills (installed to .[agents|claude]|/skills/)
  skills/          # Your skills
  skills-lock.json # devroot skills + third party skills
```

> 💡  On a Mac or dev server, a devroot checkout is typically located at `~/devroot`. But you can also place it wherever you want it.
>
> ```sh
>  ~/Development/                 # Finder-friendly
>  ~/devroot-workers/mac-mini-01/ # e.g. A Cursor agent worker
> ```

#### The vault

`devroot/vault/` (created with `/setup-vault`) is the knowledge graph for the workspace — a shared brain for work that spans sessions, checkouts, and harnesses.

`vault` is only written to on purpose, via `/plan` or `/reflect` (triggered at the end of sessions). Most tasks add nothing.

**Default behaviour**: `vault` is backed by a separate private Git repo. This serves two purposes:

- **Explicitly private**: In case you share your `devroot` with others (as I did when developing it), the more personal content of `vault` (e.g. future plans, or anything that is your private personal alpha) is kept separate.
- **"Live"-synced**: The vault repo is treated as a linear content store. Every write is committed and pushed as it happens. Agents across machines see the same record.

#### Vault layout

It may be helpful to open `vault` in a separate Markdown editor, such as [Obsidian](https://obsidian.md/) or [Cogito](https://cogito.md/). Also, you can ask non-code harnesses such as Claude on web or Grok Bot to directly read/write the vault when needed.

```sh
vault/
  memory/              # Workspace + per-repo knowledge (/reflect)
  plans/               # Numbered plans (/plan)
  index.md             # Index of all content
  USER.md              # Optional: who you are and your goals, loaded into every session
  COMMITTING_POLICY.md # How sessions commit here\

  # ⬆️ Customize later as needed (e.g. docs/, goals/, rfcs/, epics/)
```

> 💡 Unlike approaches such as [pstack](https://github.com/cursor/plugins/tree/main/pstack/skills), which write learnings into the target repo, devroot keeps memory in an *overlay* outside each repo. This is less invasive for shared projects, and leaves room to iterate on prototype skills or individual knowledge.

## Workflow

### Just code

Working in devroot, driven by an agent, should just work™, based on the `AGENTS.md` and skills, which encode the developer workflow.

And, with the **vault**, your workflow should **improve over time**, allowing you to do more things unattended. For example, cloning a target repo should be a one-time action, and then can be inferred from a plan/memory.

### Making and submitting changes

By default, devroot is set up for **deep local work** with nothing committed by default (this can be changed with the `/autopilot` skill, and for cloud agents).

Commits are made via natural language, which could be as small a nudge as: "build a commit stack that achieves X".

The flow should feel familiar:

- **Plan → Commit → Verify → Submit**  
- **(→ Amend → Resubmit)** — as needed for PR feedback.

**Submitting commits** (creating PRs) via the `/submit` skill is a more opinionated step where devroot adds some useful wiring.

- Local commits are built with `/commit-msg` and `/test-plan`, skills which build a high quality commit message body with the template `## Summary`, `## Test Plan` (separating motivation from evidence).
- When submitted, each commit body is lifted to the created Pull Request (local commits become title-only). Given your agent has an authenticated `gh` CLI, the remote PR then becomes the source of truth for subsequent description edits.

#### Optional: Using Sapling

This workflow works particularly well with [Sapling](https://sapling-scm.com/), offering [ISL](https://marketplace.visualstudio.com/items?itemName=meta.sapling-scm) in VS Code and local stacked commits. To switch, clone a target project using `--sapling`; agents can remember this via `vault`.

#### Autopilot mode

With `/autopilot` (intended for faster/private projects without formal PRs):

- **Ask → (Commits) → Push**

## User reference

#### Setup script

```sh
# Batteries included - clones <user>/dotfiles and <user>/vault if they exist
./scripts/setup --user huntie

# Workspace only, use /setup-vault later
./scripts/setup --bare
```

Ensures:

- The `dotfiles` checkout exists, if available.
- The `vault` checkout exists, if available. This repo is assumed to be private, so where nothing can authenticate it is skipped with a note and `setup` exits 3.
- The VS Code workspace file is generated.
- The `dev` shell function is installed. Skipped in a non-interactive shell. This allows you to use `dev <script>` from any directory in fish, bash or zsh.

Everything else, including the committed agent skills, works from a bare clone by design.

#### Other scripts (`dev` CLI)

```sh
# Index of all scripts
dev help

# Checkout/update a repo in projects/
dev checkout expo/expo

# Inside a checkout, pull git/sl commits, parking any WIP work
dev pull

# Symlinks local skills to .agents|claude/
dev sync-skills

# Allows agents to upload screenshots to GitHub to support PRs
dev gh-upload
```

#### Included skills

Here are the skills you might choose to manually invoke:

| Skill | Description |
|---|---|
| `/autopilot` | Enter autopilot mode. Commit each unit of work as soon as it is proven, without asking. |
| `/handoff` | Save a session handoff to resume in another agent. |
| `/plan` | Start a plan (saved to vault). |
| `/recall` | Rebuild a past session from history/vault context. |
| `/reflect` | Suggest learnings to save to skills/vault memory. |
| `/submit`, `/submit-stack` | Open the current commit, or stack of commits, as GitHub PRs. Local commit bodies are lifted into each remote PR, which becomes the source of truth thereafter. |
| `/trace-flow` | Add tagged log statements to systematically trace an end-to-end flow. |

Refer to the full [`huntie/skills`](https://github.com/huntie/skills) list in `skills-lock.json` to inspect all skills, which you can fork to `devroot/skills/` by copying!
