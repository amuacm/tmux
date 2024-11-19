# introduction to tmux

`tmux` (Terminal Multiplexer) is a powerful tool that allows users to manage multiple terminal sessions within a single terminal window. With `tmux`, you can split windows into panes, create multiple windows within a session, detach and reattach sessions, and more. This guide will walk you through some of the basic commands to get started.

## installation

```bash
sudo apt update
sudo apt install tmux
```

## starting tmux

To start a new `tmux` session, simply type:
```bash
tmux
```

Or, to start a session with a name:
```bash
tmux new -s <session_name>
```

## basic commands

`tmux` commands are usually prefixed with `Ctrl + b`, followed by another key or sequence.

### session commands

- **Detach from Session**: `Ctrl + b d`
  - Detaches the current session, allowing you to leave it running in the background.
  
- **List Sessions**: `tmux ls`
  - Lists all active `tmux` sessions.

- **Attach to Session**: `tmux attach -t <session_name>`
  - Reattaches to a previously detached session.

- **Kill a Session**: `tmux kill-session -t <session_name>`
  - Terminates a specific session by name.

### window commands

Windows are like tabs within a `tmux` session.

- **New Window**: `Ctrl + b c`
  - Creates a new window in the current session.

- **Switch to Next Window**: `Ctrl + b n`
  - Moves to the next window.

- **Switch to Previous Window**: `Ctrl + b p`
  - Moves to the previous window.

- **Rename Window**: `Ctrl + b ,`
  - Allows you to rename the current window.

- **Close Window**: `Ctrl + b &`
  - Closes the current window.

### pane commands

Panes allow you to split windows into multiple sections.

- **Split Horizontally**: `Ctrl + b %`
  - Splits the window into two panes side-by-side.

- **Split Vertically**: `Ctrl + b "`
  - Splits the window into two panes, stacked on top of each other.

- **Switch Panes**: `Ctrl + b <arrow keys>`
  - Moves to another pane in the specified direction.

- **Resize Pane**: `Ctrl + b <arrow keys>`
  - Hold `Ctrl + b` and press the arrow keys to resize the current pane.

- **Close Pane**: `Ctrl + b x`
  - Closes the current pane.

### layouts

You can arrange panes into different layouts for better visibility.

- **Even-Horizontal Layout**: `Ctrl + b : select-layout even-horizontal`
  - All panes are arranged horizontally with equal width.

- **Even-Vertical Layout**: `Ctrl + b : select-layout even-vertical`
  - All panes are arranged vertically with equal height.

- **Main-Horizontal Layout**: `Ctrl + b : select-layout main-horizontal`
  - One large pane on top, with smaller panes stacked below.

- **Main-Vertical Layout**: `Ctrl + b : select-layout main-vertical`
  - One large pane on the left, with smaller panes on the right.

- **Cycle Through Layouts**: `Ctrl + b Space`
  - Cycles through available layouts for the current window.

### copy mode

`tmux` has a copy mode for navigating and copying text within the session.

- **Enter Copy Mode**: `Ctrl + b [`
  - Allows you to scroll through the terminal output using arrow keys.

- **Copy Text**: 
  - Use `Space` to start selection, arrow keys to expand, and `Enter` to copy.

- **Paste Text**: `Ctrl + b ]`
  - Pastes the copied text in the current pane.

## basic configuration

To customize `tmux`, you can create a `~/.config/tmux/tmux.conf` file in your config directory. Here’s an example configuration with some useful settings:

```bash
# Use vi-style keybindings in copy mode
setw -g mode-keys vi

# Start windows and panes with indexing from 1 instead of 0
set -g base-index 1
setw -g pane-base-index 1

# binds
bind -n C-L select-pane -L
bind -n C-H select-pane -R
bind -n C-K select-pane -U
bind -n C-J select-pane -D

# new panes
bind c new-window -c "#{pane_current_path}"
bind '-' split-window -c "#{pane_current_path}"
bind '\' split-window -h -c "#{pane_current_path}"
```

To apply changes, either restart `tmux` or reload the configuration file with:
```bash
tmux source-file ~/.config/tmux/tmux.conf
```

## poweruser configurations

There are many ways to further customize tmux to look **really** pretty. Experiment with any plugin managers and themes you like, but this one is a good way to get started.

Start by cloning these repositories into your `~/.config/tmux` folder.
```
git clone https://github.com/tmux-plugins/tpm plugins/tpm
git clone https://github.com/dracula/tmux plugins/tmux
```

Next, update your `tmux.conf` with the following lines:
```
# basic
setw -g mode-keys vi
set-option -g status-position top

# plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'dracula/tmux'
set -g @dracula-plugins "git battery time" 
set -g @dracula-show-left-icon session
set -g @dracula-show-empty-plugins false

# binds
bind -n C-L select-pane -L
bind -n C-H select-pane -R
bind -n C-K select-pane -U
bind -n C-J select-pane -D

# new panes
bind c new-window -c "#{pane_current_path}"
bind '-' split-window -c "#{pane_current_path}"
bind '\' split-window -h -c "#{pane_current_path}"

run -b '~/.config/tmux/plugins/tpm/tpm'
```

Finally, source the file again to get a new feel for tmux! (Follow the git links to find more options for customization)
