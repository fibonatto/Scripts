# Tmux & Git Workflow Scripts

A collection of Zsh scripts designed to automate Tmux session management, Git branch creation, and selective session restoration.

## Dependencies

* `zsh`
* `tmux`
* `git`
* `neovim` (required by `dev`)
* `tmux-resurrect` (required by `tn`)

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

* standard `tmux-resurrect` behavior restores all saved sessions globally. This script parses the `last` snapshot state using `awk`, extracts only the `pane` and `window` entries corresponding to the requested session, and isolates them in a temporary directory.
* It then forces the `restore.sh` script to read from this temporary location, effectively restoring only the target session without cluttering the Tmux server with the rest of the snapshot data.
* Switches or attaches to the session upon successful restoration.

### `tmux-kill`

Silently terminates the Tmux server and all active sessions (`tmux kill-server 2>/dev/null`).

## Installation

Ensure the scripts are executable and located in a directory included in your `$PATH`:

```bash
chmod +x dev feat tn tmux-kill

```
