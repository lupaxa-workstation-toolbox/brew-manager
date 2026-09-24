# Reference

One action flag per invocation. `--yes` / `-y` and an optional `--export` path
may accompany that action. A second action flag, an unknown flag, or `--yes`
with no action prints usage and exits 2.

## Menu Options

| Option | What it runs                   | Notes                                                           |
| :----- | :----------------------------- | :-------------------------------------------------------------- |
| `1`    | `brew update`                  | Refresh Homebrew and formula/cask metadata                      |
| `2`    | `brew outdated`                | List outdated formulae                                          |
| `3`    | `brew outdated --cask`         | List outdated casks                                             |
| `4`    | `brew upgrade`                 | Confirms before upgrading formulae                              |
| `5`    | `brew upgrade --cask`          | Confirms before upgrading casks                                 |
| `6`    | `brew upgrade --cask --greedy` | Confirms before a greedy cask upgrade                           |
| `7`    | `brew list --formula`          | List installed formulae                                         |
| `8`    | `brew list --cask`             | List installed casks                                            |
| `9`    | *(custom)*                     | `brew leaves`, then `brew autoremove --dry-run`                 |
| `10`   | *(custom)*                     | Scan installed casks via Homebrew JSON                          |
| `11`   | `brew uninstall --cask NAME`   | Prompts for the cask name, then confirms                        |
| `12`   | *(custom)*                     | Lists matching casks, then confirms a bulk uninstall            |
| `13`   | `brew autoremove --dry-run`    | Preview unused dependency removals                              |
| `14`   | `brew autoremove`              | Confirms before removing unused dependencies                    |
| `15`   | `brew cleanup --dry-run`       | Preview cache and old-version cleanup                           |
| `16`   | `brew cleanup`                 | Confirms before cleaning up                                     |
| `17`   | `brew doctor`                  | Run Homebrew diagnostics                                        |
| `18`   | *(custom)*                     | Write formulae and casks under `./exports/`                     |
| `19`   | *(custom)*                     | Safe maintenance sequence (see [Usage](usage.md))               |
| `Q`    | —                              | Quit                                                            |

## Flags

| Flag                        | Menu | Notes                                                                 |
| :-------------------------- | :--- | :-------------------------------------------------------------------- |
| `-h`, `--help`              | —    | Print usage and exit 0. Does not require `brew`                       |
| `--update`                  | 1    |                                                                       |
| `--outdated`                | 2    |                                                                       |
| `--outdated-cask`           | 3    |                                                                       |
| `--upgrade`                 | 4    | Requires `--yes`                                                      |
| `--upgrade-cask`            | 5    | Requires `--yes`                                                      |
| `--upgrade-cask-greedy`     | 6    | Requires `--yes`                                                      |
| `--list-formula`            | 7    |                                                                       |
| `--list-cask`               | 8    |                                                                       |
| `--leaves-preview`          | 9    |                                                                       |
| `--list-deprecated`         | 10   |                                                                       |
| `--uninstall-cask NAME`     | 11   | Requires `--yes`. `NAME` must not start with `-`                      |
| `--remove-deprecated-casks` | 12   | Requires `--yes`                                                      |
| `--autoremove-dry-run`      | 13   |                                                                       |
| `--autoremove`              | 14   | Requires `--yes`                                                      |
| `--cleanup-dry-run`         | 15   |                                                                       |
| `--cleanup`                 | 16   | Requires `--yes`                                                      |
| `--doctor`                  | 17   |                                                                       |
| `--export [PATH]`           | 18   | Default `./exports/brew-list-YYYYMMDD-HHMMSS.txt`                     |
| `--safe-run`                | 19   | Same sequence as option 19                                            |
| `-y`, `--yes`               | —    | Required with a destructive flag. Flag mode never prompts             |

## Exit Status

| Status | When                                                                                               |
| :----- | :------------------------------------------------------------------------------------------------- |
| `0`    | Help, quit, end of file, or the action finished without a reported failure                         |
| `1`    | `brew` is not on `PATH`, or an export could not be created or written                              |
| `2`    | Unknown flag, missing cask name, a second action, no action, or a destructive flag without `--yes` |
| other  | The underlying `brew` command's status, in flag mode                                               |

A missing `--yes` prints `Error: destructive action requires --yes` on stderr.
Menu mode prints a `brew` failure and returns to the menu instead of exiting
with that status.

## Export File

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

On success the script prints `Wrote PATH`. Failure to create the directory,
the temporary file, or the `brew list` output exits 1 (and, in the menu,
pauses before returning).

## Deprecated and Disabled Casks

Options `10` and `12`, and `--list-deprecated` / `--remove-deprecated-casks`,
read `brew info --json=v2 --cask`. A cask matches when that JSON contains
`"deprecated": true` or `"disabled": true`. Free-text `brew info` output is
not parsed. When nothing matches, the list command prints `None found.`
