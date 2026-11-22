# Install devroot

This guide is for a coding agent. Follow it to create a devroot for the user you are working with.

A devroot is a workspace repository that agent sessions run from: a few scripts, an `AGENTS.md` that says how to work, and the skills under its agent directories, with project checkouts, dotfiles and a vault beside them. The mechanics live in [`install.sh`](https://raw.githubusercontent.com/huntie/setup-devroot/main/install.sh); the steps below are the questions and judgment around it.

## Setup

Work through these steps in order.

### 1. Check the tools

The install needs `git`, `npx` and the GitHub CLI, signed in: `gh auth status`. Without `gh`, or signed out, stop and give the install and `gh auth login` steps; the template is fetched through it, and the scripts in the workspace use it.

### 2. Ask where the devroot goes

Suggest `~/devroot`, and let the user choose another path or name. The directory must be new or empty. Confirm the path.

### 3. Ask which GitHub user it is for

Suggest the login from `gh api user --jq .login`. Explain the two optional repositories the workspace brings in under that name, both gitignored and each a repository of its own:

- `<user>/dotfiles`, cloned to `dotfiles/`: their dotfiles and the agent conventions, such as a `CLAUDE.md`, that apply across the workspace.
- `<user>/vault` (recommended), private, cloned to `vault/`: where plans and working knowledge accumulate across sessions and machines.

Most users have neither yet, and that is the expected case: setup notes what it could not find, dotfiles can be added at any time, and the vault is offered in step 5.

### 4. Run the install script

Fetch the script to a temporary directory and read its `--help`:

```sh
curl -fsSL https://raw.githubusercontent.com/huntie/setup-devroot/main/install.sh -o "$TMPDIR/devroot-install.sh"
bash "$TMPDIR/devroot-install.sh" --help
```

Run it with `--dry-run`, the path and `--user`, and report what it will do. Then run it for real:

```sh
bash "$TMPDIR/devroot-install.sh" <path> --user <user> --dry-run
bash "$TMPDIR/devroot-install.sh" <path> --user <user>
```

The script does the following:

- Fetches `devroot-template-latest/` from `huntie/setup-devroot` at its latest commit, and records that commit in `.devroot-version`, which `/update-devroot` reads later.
- Installs the skills from `huntie/skills`, `huntie/skills-pstack-vendored` and `huntie/setup-devroot` into the workspace's agent directories.
- Writes the initial commit.
- Runs the workspace's own `scripts/setup`.

Exit 3 means setup could not clone a `dotfiles` or `vault` repository, which is expected for a new user; say which.

### 5. Offer the vault

Ask whether to set up a vault now, the private repository of plans and working knowledge the workspace reads. On yes, change into the new devroot and follow `.agents/skills/setup-vault/SKILL.md`. Otherwise say it can be done later with `/setup-vault` from inside the workspace.

### 6. Report

End with the path, the skills installed, and what setup skipped. Point the user to `README.md` in the new devroot for getting started, and to `AGENTS.md` for how sessions work there. Every later session starts in the devroot.
