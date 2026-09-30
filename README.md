<p align="center">
    <a href="https://github.com/lupaxa-workstation-toolbox">
        <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/workstation-toolbox/readme-logo.png" alt="Organisation Logo" />
    </a>
</p>

<h1 align="center">Brew Manager</h1>

Interactive Homebrew maintenance menu for workstation brew state. Run common
`brew` operations from a numbered menu — updates, upgrades, cleanup, doctor,
installed package lists, and helpers for deprecated or disabled casks.

Destructive actions ask for confirmation in menu mode. Dry-run options are
available where Homebrew supports them. Non-interactive CLI flags mirror menu
actions for scripting. Flag mode never prompts.

## Requirements

- [Homebrew](https://brew.sh) installed and available on `PATH`
- Bash (macOS `/bin/bash` 3.2 or newer is fine)
- A terminal (the menu clears the screen and pauses after each command)

## Quick Start

With Homebrew:

```bash
brew tap the-lupaxa-project/tap
brew trust the-lupaxa-project/tap
brew install brew-manager
```

Or clone the repository and run the script:

```bash
git clone git@github.com:lupaxa-workstation-toolbox/brew-manager.git
cd brew-manager
./src/brew-manager
```

To run a checkout from anywhere on `PATH`, copy or symlink into your personal `bin`:

```bash
cp /path/to/brew-manager/src/brew-manager ~/bin/brew-manager
chmod +x ~/bin/brew-manager
```

With no arguments the interactive menu opens. Press `Q` to quit.

```bash
./src/brew-manager --help
```

`--help` prints the flags and exits 0; it does not require `brew` on `PATH`.
Any other invocation does: if `brew` is missing, the script prints
`Error: brew was not found in PATH.` and exits 1 before showing the menu.

## Modes

**Menu mode** is the default (no arguments). The screen clears, you pick a
number, and after most commands you press Enter to return. Destructive options
ask `[y/N]` first. The default answer is no.

**Flag mode** is one action flag per run. There is no menu, no pause, and no
confirmation prompt. Destructive flags exit 2 unless you also pass `-y` or
`--yes`.

## Typical Workflow

1. Refresh Homebrew metadata and see what is outdated.
2. Preview cleanup. Nothing is removed.
3. Upgrade or clean up only after you have read the preview.

```bash
./src/brew-manager --safe-run
./src/brew-manager --outdated
./src/brew-manager --upgrade --yes
```

The same sequence from the menu is option `19`, then `2`, then `4`.

## Interactive Menu

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

`Q`, `q`, or `quit` prints `Goodbye.` and exits 0. End of file on the menu
(or on the Enter pause) prints `EOF; exiting.` and exits 0. Any other text
prints `Invalid option:` and returns to the menu.

A failing `brew` command prints `Command exited with status N.` The menu
stays open.

### Menu Reference

| Option | What it runs                   | Notes                                           |
| :----- | :----------------------------- | :---------------------------------------------- |
| `1`    | `brew update`                  | Refresh Homebrew and formula/cask metadata      |
| `2`    | `brew outdated`                | List outdated formulae                          |
| `3`    | `brew outdated --cask`         | List outdated casks                             |
| `4`    | `brew upgrade`                 | Confirms before upgrading formulae              |
| `5`    | `brew upgrade --cask`          | Confirms before upgrading casks                 |
| `6`    | `brew upgrade --cask --greedy` | Confirms before a greedy cask upgrade           |
| `7`    | `brew list --formula`          | List installed formulae                         |
| `8`    | `brew list --cask`             | List installed casks                            |
| `9`    | *(custom)*                     | `brew leaves`, then `brew autoremove --dry-run` |
| `10`   | *(custom)*                     | Scan installed casks via Homebrew JSON          |
| `11`   | `brew uninstall --cask NAME`   | Prompts for the cask name, then confirms        |
| `12`   | *(custom)*                     | Lists matching casks, then confirms bulk remove |
| `13`   | `brew autoremove --dry-run`    | Preview unused dependency removals              |
| `14`   | `brew autoremove`              | Confirms before removing unused dependencies    |
| `15`   | `brew cleanup --dry-run`       | Preview cache and old-version cleanup           |
| `16`   | `brew cleanup`                 | Confirms before cleaning up                     |
| `17`   | `brew doctor`                  | Run Homebrew diagnostics                        |
| `18`   | *(custom)*                     | Write formulae and casks under `./exports/`     |
| `19`   | *(custom)*                     | Safe maintenance sequence (see below)           |
| `Q`    | —                              | Quit                                            |

### Full Safe Maintenance Run (Option 19)

Option `19` and `--safe-run` run this sequence, in order:

1. `brew update`
2. `brew outdated`
3. `brew outdated --cask`
4. `brew autoremove --dry-run`
5. `brew cleanup --dry-run`
6. `brew doctor`

Nothing is upgraded or removed. In menu mode the script pauses after each
command. The run's exit status is the status of the last command that failed,
or 0 when every command succeeded.

## CLI Flags

One action flag per invocation. `--yes` / `-y` and an optional `--export` path
may accompany that action. A second action flag, an unknown flag, or `--yes`
with no action prints usage and exits 2.

| Flag                        | Menu | Notes                                                     |
| :-------------------------- | :--- | :-------------------------------------------------------- |
| `-h`, `--help`              | —    | Print usage and exit 0. Does not require `brew`           |
| `--update`                  | 1    |                                                           |
| `--outdated`                | 2    |                                                           |
| `--outdated-cask`           | 3    |                                                           |
| `--upgrade`                 | 4    | Requires `--yes`                                          |
| `--upgrade-cask`            | 5    | Requires `--yes`                                          |
| `--upgrade-cask-greedy`     | 6    | Requires `--yes`                                          |
| `--list-formula`            | 7    |                                                           |
| `--list-cask`               | 8    |                                                           |
| `--leaves-preview`          | 9    |                                                           |
| `--list-deprecated`         | 10   |                                                           |
| `--uninstall-cask NAME`     | 11   | Requires `--yes`. `NAME` must not start with `-`          |
| `--remove-deprecated-casks` | 12   | Requires `--yes`                                          |
| `--autoremove-dry-run`      | 13   |                                                           |
| `--autoremove`              | 14   | Requires `--yes`                                          |
| `--cleanup-dry-run`         | 15   |                                                           |
| `--cleanup`                 | 16   | Requires `--yes`                                          |
| `--doctor`                  | 17   |                                                           |
| `--export [PATH]`           | 18   | Default `./exports/brew-list-YYYYMMDD-HHMMSS.txt`         |
| `--safe-run`                | 19   | Same sequence as option 19                                |
| `-y`, `--yes`               | —    | Required with a destructive flag. Flag mode never prompts |

### Destructive Actions

These change the machine. In the menu they ask first. As flags they require
`--yes`.

| Menu | Flag                        | Prompt or gate                      |
| :--- | :-------------------------- | :---------------------------------- |
| `4`  | `--upgrade`                 | `Run brew upgrade?`                 |
| `5`  | `--upgrade-cask`            | `Run brew upgrade --cask?`          |
| `6`  | `--upgrade-cask-greedy`     | `Run brew upgrade --cask --greedy?` |
| `11` | `--uninstall-cask NAME`     | Asks for a cask name, then confirms |
| `12` | `--remove-deprecated-casks` | Lists matches, then confirms        |
| `14` | `--autoremove`              | `Run brew autoremove?`              |
| `16` | `--cleanup`                 | `Run brew cleanup?`                 |

Option `11` with an empty name prints `No cask entered.` and returns to the
menu. Option `12` with no matches prints `No deprecated or disabled casks
found.` and does not uninstall anything.

Flag mode prints each command as `==> …` before running it. A non-zero `brew`
status is printed and becomes the process exit status, except bulk
deprecated-cask removal, which prints each cask's status and still exits 0.

### Exit Status

| Status | When                                                                                               |
| :----- | :------------------------------------------------------------------------------------------------- |
| `0`    | Help, quit, end of file, or the action finished without a reported failure                         |
| `1`    | `brew` is not on `PATH`, or an export could not be created or written                              |
| `2`    | Unknown flag, missing cask name, a second action, no action, or a destructive flag without `--yes` |
| other  | The underlying `brew` command's status, in flag mode                                               |

A missing `--yes` prints `Error: destructive action requires --yes` on stderr.

### Export File

Option `18` always writes the default path. `--export` uses that default when
no path follows it. A following argument that starts with `-` is left for flag
parsing, so the default path is used.

The file is written via a temporary file in the same directory, then renamed.
Contents:

```text
# Formulae
<one formula name per line>

# Casks
<one cask name per line>
```

On success the script prints `Wrote PATH`.

### Deprecated and Disabled Casks

Options `10` and `12`, and `--list-deprecated` / `--remove-deprecated-casks`,
read `brew info --json=v2 --cask`. A cask matches when that JSON contains
`"deprecated": true` or `"disabled": true`. Free-text `brew info` output is
not parsed. When nothing matches, the list command prints `None found.`

## Examples

**Routine check (no changes):**

```bash
./src/brew-manager --safe-run
# or menu option 19
```

**Upgrade formulae after reviewing outdated:**

```bash
./src/brew-manager --outdated
./src/brew-manager --upgrade --yes
```

`./src/brew-manager --upgrade` without `--yes` prints
`Error: destructive action requires --yes` and exits 2.

**Find and remove deprecated casks:**

```bash
./src/brew-manager --list-deprecated
./src/brew-manager --uninstall-cask SOME-CASK --yes
./src/brew-manager --remove-deprecated-casks --yes
```

**Preview cleanup without deleting anything:**

```bash
./src/brew-manager --autoremove-dry-run
./src/brew-manager --cleanup-dry-run
```

**Export installed lists:**

```bash
./src/brew-manager --export
./src/brew-manager --export ~/backups/brew-list.txt
```

**Refused invocations** (each exits 2):

```bash
./src/brew-manager --upgrade --cleanup --yes
./src/brew-manager --not-a-flag
./src/brew-manager --yes
```

Only one action flag is accepted. `--yes` on its own is not an action.

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
