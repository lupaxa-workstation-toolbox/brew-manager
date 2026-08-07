<p align="center">
    <a href="https://github.com/lupaxa-workstation-toolbox">
        <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/workstation-toolbox/readme-logo.png" alt="Organisation Logo" />
    </a>
</p>

<h1 align="center">brew-manager</h1>

Interactive Homebrew maintenance menu for workstation brew state. Run common
`brew` operations from a numbered menu — updates, upgrades, cleanup, doctor,
installed package lists, and helpers for deprecated or disabled casks.

Destructive actions (upgrade, autoremove, cleanup, uninstall) ask for
confirmation first. Dry-run options are available where Homebrew supports them.

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

Or run it from anywhere once cloned:

```bash
/path/to/brew-manager/src/brew-manager
```

If `brew` is missing from `PATH`, the script exits with an error before showing
the menu.

## CLI usage

`brew-manager` is an interactive TUI-style menu. There are no subcommands or
flags — start the script, pick a number (or `Q` to quit), and follow the prompts.

```text
Homebrew Maintenance Menu
=========================

  1) brew update
  2) brew outdated
  3) brew outdated --cask
  4) brew upgrade
  5) brew upgrade --cask
  6) brew autoremove --dry-run
  7) brew autoremove
  8) brew cleanup --dry-run
  9) brew cleanup
 10) brew doctor
 11) list installed formulae
 12) list installed casks
 13) list deprecated/disabled casks
 14) uninstall a cask
 15) remove all deprecated/disabled casks
 16) full safe maintenance run

  Q) quit

Select an option:
```

After most commands finish, press **Enter** to return to the menu.

### Menu reference

| Option | What it runs                   | Notes                                                                  |
| :----- | :----------------------------- | :--------------------------------------------------------------------- |
| `1`    | `brew update`                  | Refresh Homebrew and formula/cask metadata                             |
| `2`    | `brew outdated`                | List outdated formulae                                                 |
| `3`    | `brew outdated --cask`         | List outdated casks                                                    |
| `4`    | `brew upgrade`                 | Confirms before upgrading formulae                                     |
| `5`    | `brew upgrade --cask`          | Confirms before upgrading casks                                        |
| `6`    | `brew autoremove --dry-run`    | Preview unused dependency removals                                     |
| `7`    | `brew autoremove`              | Confirms before removing unused deps                                   |
| `8`    | `brew cleanup --dry-run`       | Preview cache / old-version cleanup                                    |
| `9`    | `brew cleanup`                 | Confirms before cleaning up                                            |
| `10`   | `brew doctor`                  | Run Homebrew diagnostics                                               |
| `11`   | `brew list --formula`          | List installed formulae                                                |
| `12`   | `brew list --cask`             | List installed casks                                                   |
| `13`   | *(custom)*                     | Scan installed casks for `deprecated` / `disabled` in `brew info`      |
| `14`   | `brew uninstall --cask <name>` | Prompts for cask name, then confirms                                   |
| `15`   | *(custom)*                     | Lists matching deprecated/disabled casks, then confirms bulk uninstall |
| `16`   | *(custom)*                     | Safe maintenance sequence (see below)                                  |
| `Q`    | —                              | Quit                                                                   |

### Full safe maintenance run (option 16)

Runs these commands in order, pausing after each:

1. `brew update`
2. `brew outdated`
3. `brew outdated --cask`
4. `brew autoremove --dry-run`
5. `brew cleanup --dry-run`
6. `brew doctor`

Nothing is upgraded or removed — only update, inspect, and dry-run cleanup.

## Examples

**Routine check (no changes):**

```bash
./src/brew-manager
# choose 16 — full safe maintenance run
# or step through 1 → 2 → 3 → 6 → 8 → 10
```

**Upgrade formulae after reviewing outdated:**

```bash
./src/brew-manager
# 2  — review outdated formulae
# 4  — confirm brew upgrade when ready
```

**Find and remove deprecated casks:**

```bash
./src/brew-manager
# 13 — list deprecated/disabled installed casks
# 14 — uninstall one by name
# 15 — uninstall all listed deprecated/disabled casks (with confirmation)
```

**Preview cleanup without deleting anything:**

```bash
./src/brew-manager
# 6 — autoremove dry-run
# 8 — cleanup dry-run
```

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
