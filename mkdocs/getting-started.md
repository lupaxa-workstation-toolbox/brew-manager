# Getting Started

## Requirements

- [Homebrew](https://brew.sh) installed and available on `PATH`
- Bash (macOS `/bin/bash` 3.2 or newer is fine)
- A terminal (the menu clears the screen and pauses after each command)

There is no Homebrew formula and no `make install` target in this repository.

## Install

Clone the repository, then run the script:

```bash
git clone git@github.com:lupaxa-workstation-toolbox/brew-manager.git
cd brew-manager
./src/brew-manager
```

To run it from anywhere on `PATH`, copy or symlink it into a personal `bin`:

```bash
cp /path/to/brew-manager/src/brew-manager ~/bin/brew-manager
chmod +x ~/bin/brew-manager
```

## First Run

With no arguments the interactive menu opens. Press `Q` to quit.

```bash
./src/brew-manager --help
```

You should see the flag list and the process should exit 0. `--help` does not
require `brew` to be on `PATH`. Any other invocation does: if `brew` is
missing, the script prints `Error: brew was not found in PATH.` and exits 1
before showing the menu.

## Documentation Site

Site pages live in `mkdocs/`. Serve them locally:

```bash
python -m pip install -r requirements.txt
python -m mkdocs serve
```

Open the URL MkDocs prints (usually `http://127.0.0.1:8000/`).

With the `mkdocs` makefile skill enabled, `make update` then
`make mkdocs-serve` serves the same site.
