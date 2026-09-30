# Tmux, Git & Shell Utility Scripts

A collection of Zsh scripts designed to automate Tmux session management, Git branch creation, selective session restoration, and a few small shell utilities.

## Dependencies

* `zsh`
* `tmux`
* `git`
* `neovim` (required by `dev`)
* `tmux-resurrect` (required by `tn`)
* `w3m` and `python3` (required by `ddg`)
* `unrar` / `7z` (optional, only for `.rar` / `.7z` in `extract`)

## Scripts Overview

### `dev`

Initializes or attaches to a Tmux session named after the current working directory. Upon creation, it configures a predefined split layout:

* Left pane (full height): Opens `nvim .`
* Right pane (split vertically): Provides two separate shell prompts.

### `feat <branch>`

Automates the setup of a new Git branch alongside an isolated Tmux session.

* Validates if the requested branch name already exists locally or remotely (`origin`).
* If a collision occurs, it interactively prompts the user for a valid alternative name.
* Initializes a Git repository if executed outside of an existing working tree.
* Creates or switches to a Tmux session named after the branch.

### `tn <session>`

Selectively restores a single Tmux session from a `tmux-resurrect` snapshot.

* Standard `tmux-resurrect` behavior restores all saved sessions globally. This script parses the `last` snapshot state using `awk`, extracts only the `pane` and `window` entries corresponding to the requested session, and isolates them in a temporary directory.
* It then forces the `restore.sh` script to read from this temporary location, effectively restoring only the target session without cluttering the Tmux server with the rest of the snapshot data.
* Switches or attaches to the session upon successful restoration.

### `tmux-kill`

Silently terminates the Tmux server and all active sessions (`tmux kill-server 2>/dev/null`).

### `ddg <query>`

Opens a DuckDuckGo search for the given query in `w3m`. The query is URL-encoded with Python. Intended to be aliased with `noglob` so `?` and `*` pass through the shell untouched:

```zsh
alias '?'='noglob ddg'
```

### `killp <process_name>`

Kills all processes matching the given name (`pgrep` check first, then `pkill`).

### `extract <archive>`

Extracts an archive into the current directory, picking the tool by extension: `.tar.bz2`, `.tbz2`, `.tar.gz`, `.tgz`, `.tar`, `.bz2`, `.gz`, `.zip`, `.Z`, `.rar`, `.7z`. Fails with a clear message if the format is unknown or the required tool is missing.

## Installation

Ensure the scripts are executable and located in a directory included in your `$PATH`:

```bash
chmod +x dev feat tn tmux-kill ddg killp extract
```
