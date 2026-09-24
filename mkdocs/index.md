# Brew Manager

Interactive Homebrew maintenance menu for workstation brew state. Run common
`brew` operations from a numbered menu: updates, upgrades, cleanup, doctor,
installed package lists, and helpers for deprecated or disabled casks.

Destructive actions ask for confirmation in menu mode. Dry-run options are
available where Homebrew supports them. Non-interactive flags mirror the menu
for scripting. Flag mode never prompts.

## What You Get

- A numbered menu for the usual Homebrew maintenance commands
- Matching flags so the same actions can run from a script
- A `--yes` gate on anything that upgrades or removes packages
- A safe run that only updates metadata, lists outdated packages, and dry-runs cleanup
- An export of installed formulae and casks
