<p align="center">
    <a href="https://github.com/lupaxa-workstation-toolbox">
        <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/workstation-toolbox/readme-logo.png" alt="Organisation Logo" />
    </a>
</p>

<h1 align="center">brew-manager</h1>

Interactive Homebrew maintenance menu for workstation brew state. Run common
`brew` operations from a numbered menu — updates, upgrades, cleanup, doctor,
installed package lists, and helpers for deprecated or disabled casks.

Destructive actions ask for confirmation in menu mode. Dry-run options are
available where Homebrew supports them. Non-interactive CLI flags mirror menu
actions for scripting.

## Requirements

- [Homebrew](https://brew.sh) installed and available on `PATH`
- Bash
- A terminal (the menu clears the screen and pauses after each command)

## Quick start

Clone the repository, then run the script:

```bash
git clone git@github.com:lupaxa-workstation-toolbox/brew-manager.git
cd brew-manager
./src/brew-manager
```

To run it from anywhere on `PATH`, copy or symlink into your personal `bin`
(no Makefile install or Homebrew formula in this repo):

```bash
cp /path/to/brew-manager/src/brew-manager ~/bin/brew-manager
chmod +x ~/bin/brew-manager
```

If `brew` is missing from `PATH`, the script exits with an error before showing
the menu.

## Interactive menu

With no arguments, `brew-manager` opens an interactive menu. Pick a number (or
`Q` to quit) and follow the prompts.

```text
Homebrew Maintenance Menu
=========================

  1) brew update
  2) brew outdated
  3) brew outdated --cask
  4) brew upgrade
  5) brew upgrade --cask
  6) brew upgrade --cask --greedy
  7) list installed formulae
  8) list installed casks
  9) leaves / orphan preview
 10) list deprecated/disabled casks
 11) uninstall a cask
 12) remove all deprecated/disabled casks
 13) brew autoremove --dry-run
 14) brew autoremove
 15) brew cleanup --dry-run
 16) brew cleanup
 17) brew doctor
 18) export installed lists
 19) full safe maintenance run

  Q) quit

Select an option:
```

After most commands finish, press **Enter** to return to the menu.

### Menu reference

| Option | What it runs                          | Notes                                                                  |
| :----- | :------------------------------------ | :--------------------------------------------------------------------- |
| `1`    | `brew update`                         | Refresh Homebrew and formula/cask metadata                             |
| `2`    | `brew outdated`                       | List outdated formulae                                                 |
| `3`    | `brew outdated --cask`                | List outdated casks                                                    |
| `4`    | `brew upgrade`                        | Confirms before upgrading formulae                                     |
| `5`    | `brew upgrade --cask`                 | Confirms before upgrading casks                                        |
| `6`    | `brew upgrade --cask --greedy`        | Confirms before greedy cask upgrade                                    |
| `7`    | `brew list --formula`                 | List installed formulae                                                |
| `8`    | `brew list --cask`                    | List installed casks                                                   |
| `9`    | *(custom)*                            | `brew leaves`, then `brew autoremove --dry-run` (informational)        |
| `10`   | *(custom)*                            | Scan installed casks via Homebrew JSON (see below)                     |
| `11`   | `brew uninstall --cask <name>`        | Prompts for cask name, then confirms                                   |
| `12`   | *(custom)*                            | Lists matching deprecated/disabled casks, then confirms bulk uninstall |
| `13`   | `brew autoremove --dry-run`           | Preview unused dependency removals                                     |
| `14`   | `brew autoremove`                     | Confirms before removing unused deps                                   |
| `15`   | `brew cleanup --dry-run`              | Preview cache / old-version cleanup                                    |
| `16`   | `brew cleanup`                        | Confirms before cleaning up                                            |
| `17`   | `brew doctor`                         | Run Homebrew diagnostics                                               |
| `18`   | *(custom)*                            | Export formulae and casks to `./exports/` or a chosen path             |
| `19`   | *(custom)*                            | Safe maintenance sequence (see below)                                  |
| `Q`    | —                                     | Quit                                                                   |

### Deprecated / disabled cask detection

Options **10** and **12** (and `--list-deprecated` / `--remove-deprecated-casks`)
identify casks using `brew info --json=v2 --cask`. A cask is treated as
deprecated or disabled when the JSON contains `"deprecated": true` or
`"disabled": true`. The script does not parse free-text `brew info` output.

### Full safe maintenance run (option 19)

Runs these commands in order, pausing after each in menu mode:

1. `brew update`
2. `brew outdated`
3. `brew outdated --cask`
4. `brew autoremove --dry-run`
5. `brew cleanup --dry-run`
6. `brew doctor`

Nothing is upgraded or removed — only update, inspect, and dry-run cleanup.

## CLI usage

Pass one action flag to run non-interactively (no menu, no pauses). Use
`./src/brew-manager --help` for a short summary.

| Flag                        | Menu | Notes                                              |
| :-------------------------- | :--- | :------------------------------------------------- |
| `-h`, `--help`              | —    | Print usage and exit 0                             |
| `--update`                  | 1    |                                                    |
| `--outdated`                | 2    |                                                    |
| `--outdated-cask`           | 3    |                                                    |
| `--upgrade`                 | 4    | Requires `--yes`                                   |
| `--upgrade-cask`            | 5    | Requires `--yes`                                   |
| `--upgrade-cask-greedy`     | 6    | Requires `--yes`                                   |
| `--list-formula`            | 7    |                                                    |
| `--list-cask`               | 8    |                                                    |
| `--leaves-preview`          | 9    |                                                    |
| `--list-deprecated`         | 10   |                                                    |
| `--uninstall-cask NAME`     | 11   | Requires `--yes`                                   |
| `--remove-deprecated-casks` | 12   | Requires `--yes`                                   |
| `--autoremove-dry-run`      | 13   |                                                    |
| `--autoremove`              | 14   | Requires `--yes`                                   |
| `--cleanup-dry-run`         | 15   |                                                    |
| `--cleanup`                 | 16   | Requires `--yes`                                   |
| `--doctor`                  | 17   |                                                    |
| `--export [PATH]`           | 18   | Default `./exports/brew-list-YYYYMMDD-HHMMSS.txt`  |
| `--safe-run`                | 19   | Same sequence as option 19                         |
| `-y`, `--yes`               | —    | Required for destructive flags (see below)         |

**`--yes` rule:** Destructive flags (`--upgrade`, `--upgrade-cask`,
`--upgrade-cask-greedy`, `--uninstall-cask`, `--remove-deprecated-casks`,
`--autoremove`, `--cleanup`) exit with status **2** unless `-y` / `--yes` is
also passed. Flag mode never prompts for confirmation.

**One action per run:** Only one action flag is allowed per invocation (plus
optional `--yes` or an `--export` path). Unknown flags or a second action flag
print usage and exit 2.

## Examples

**Routine check (no changes):**

```bash
./src/brew-manager
# choose 19 — full safe maintenance run
# or: ./src/brew-manager --safe-run
```

**Upgrade formulae after reviewing outdated:**

```bash
./src/brew-manager
# 2  — review outdated formulae
# 4  — confirm brew upgrade when ready
# or: ./src/brew-manager --upgrade --yes
```

**Find and remove deprecated casks:**

```bash
./src/brew-manager
# 10 — list deprecated/disabled installed casks
# 11 — uninstall one by name
# 12 — uninstall all listed deprecated/disabled casks (with confirmation)
```

**Preview cleanup without deleting anything:**

```bash
./src/brew-manager
# 13 — autoremove dry-run
# 15 — cleanup dry-run
```

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
