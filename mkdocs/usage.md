# Usage

`brew-manager` has two modes.

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

## Safe Maintenance Run

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

## Destructive Actions

These change the machine. In the menu they ask first. As flags they require
`--yes`.

| Menu | Flag                        | Prompt or gate                         |
| :--- | :-------------------------- | :------------------------------------- |
| `4`  | `--upgrade`                 | `Run brew upgrade?`                    |
| `5`  | `--upgrade-cask`            | `Run brew upgrade --cask?`             |
| `6`  | `--upgrade-cask-greedy`     | `Run brew upgrade --cask --greedy?`    |
| `11` | `--uninstall-cask NAME`     | Asks for a cask name, then confirms    |
| `12` | `--remove-deprecated-casks` | Lists matches, then confirms           |
| `14` | `--autoremove`              | `Run brew autoremove?`                 |
| `16` | `--cleanup`                 | `Run brew cleanup?`                    |

Option `11` with an empty name prints `No cask entered.` and returns to the
menu. Option `12` with no matches prints `No deprecated or disabled casks
found.` and does not uninstall anything.

Flag mode prints each command as `==> …` before running it. A non-zero `brew`
status is printed and becomes the process exit status, except bulk
deprecated-cask removal, which prints each cask's status and still exits 0.
