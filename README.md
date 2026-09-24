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
the menu. `./src/brew-manager --help` prints the flags and exits 0.

## Documentation

Menu options, flags, exit statuses, and examples:

<https://brew-manager.thelupaxaproject.org/>

Site pages live in `mkdocs/`. Serve them from this checkout:

```bash
python -m pip install -r requirements.txt
mkdocs serve
```

After `make update`, `make mkdocs-serve` does the same.

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
