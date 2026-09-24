# Examples

Paths below assume you are in the repository root. The same flags work if
`brew-manager` is on `PATH`.

## Routine Check

Update metadata, list what is outdated, and dry-run cleanup. Nothing is
upgraded or removed.

```bash
./src/brew-manager --safe-run
```

From the menu, choose `19`. The script pauses after each command until you
press Enter.

## Upgrade Formulae

Review first, then upgrade:

```bash
./src/brew-manager --outdated
./src/brew-manager --upgrade --yes
```

From the menu, choose `2`, then `4`, and answer `y` to `Run brew upgrade?`.

!!! warning "Missing --yes"
    `./src/brew-manager --upgrade` prints `Error: destructive action requires --yes`
    and exits 2. No `brew upgrade` runs.

## Deprecated Casks

```bash
./src/brew-manager --list-deprecated
./src/brew-manager --uninstall-cask SOME-CASK --yes
./src/brew-manager --remove-deprecated-casks --yes
```

`--list-deprecated` prints `None found.` when every installed cask is current.
`--remove-deprecated-casks --yes` uninstalls only the names that list would
have printed. From the menu, use `10`, then `11` for one cask or `12` for all
of them. Option `12` prints the names and waits for `[y/N]` before uninstalling.

## Preview Cleanup

```bash
./src/brew-manager --autoremove-dry-run
./src/brew-manager --cleanup-dry-run
```

Menu options `13` and `15` are the same previews. Options `14` and `16`, or
`--autoremove --yes` and `--cleanup --yes`, perform the removal.

## Export Lists

```bash
./src/brew-manager --export
./src/brew-manager --export ~/backups/brew-list.txt
```

The first command writes `./exports/brew-list-YYYYMMDD-HHMMSS.txt` and prints
`Wrote` plus that path. The file has a `# Formulae` section and a `# Casks`
section. Menu option `18` always uses the default path.

## Refused Invocations

```bash
./src/brew-manager --upgrade --cleanup --yes
./src/brew-manager --not-a-flag
./src/brew-manager --yes
```

Each of these prints a short error, then the usage text, and exits 2. Only one
action flag is accepted. `--yes` on its own is not an action.
