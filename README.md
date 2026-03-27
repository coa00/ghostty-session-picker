# ghostty-session-picker

fzf-based session picker for [Ghostty](https://ghostty.org/). Switch between plain shell, tmux, and zellij sessions every time you open a new window.

![session picker](https://github.com/user-attachments/assets/placeholder)

```
 session > 
   Select a session or create new
   [new] Ghostty (plain shell)
   [new] tmux
   [new] zellij
   [zellij] my-project
   [tmux] claude-team
   [tmux] dev-server
```

## Why?

Each terminal environment has trade-offs:

| | Ghostty | Ghostty + tmux | Ghostty + zellij |
|---|:---:|:---:|:---:|
| URL click | :white_check_mark: | :x: | :x: |
| Japanese input | :white_check_mark: | :warning: | :white_check_mark: |
| Session persistence | :x: | :white_check_mark: | :white_check_mark: |
| Split panes (Claude Code Team) | :x: | :white_check_mark: | :x: |

Instead of choosing one, **pick the right one each time**.

## Requirements

- [Ghostty](https://ghostty.org/) (v1.3+)
- [fzf](https://github.com/junegunn/fzf) (required)
- [tmux](https://github.com/tmux/tmux) (optional)
- [zellij](https://zellij.dev/) (optional)

## Install

```bash
# Download the script
curl -fsSL https://raw.githubusercontent.com/coa00/ghostty-session-picker/main/ghostty-session-picker \
  -o ~/.local/bin/ghostty-session-picker
chmod +x ~/.local/bin/ghostty-session-picker
```

Add to your Ghostty config:

```
# ~/.config/ghostty/config
command = ~/.local/bin/ghostty-session-picker
```

That's it. Open a new Ghostty window and the picker will appear.

## Last working directory

New sessions can start in the directory you were last working in.

Add this to your `~/.zshrc` (or `~/.bashrc`):

```bash
# Save working directory on every cd
chpwd() {
  echo "$PWD" > ~/.last_working_dir
}
```

The picker reads `~/.last_working_dir` when creating a new session. If you're in tmux, it prefers the current path of the last active pane.

## Configuration

All configuration is via environment variables (all optional):

| Variable | Description | Default |
|---|---|---|
| `ZELLIJ_BIN` | Path to zellij binary | auto-detected |
| `TMUX_BIN` | Path to tmux binary | auto-detected |
| `FZF_BIN` | Path to fzf binary | auto-detected |
| `GHOSTTY_SESSION_PICKER_LOG` | Log file path | `~/.local/share/ghostty-session-picker.log` |

If tmux or zellij is not installed, those options are hidden from the picker.

## Behavior

- **Esc / Ctrl-C**: Cancel the picker and start a plain shell
- **`[new] Ghostty (plain shell)`**: Start a plain shell (no multiplexer)
- **`[new] tmux`**: Create a new tmux session
- **`[new] zellij`**: Create a new zellij session (prompts for name)
- **`[tmux] session-name`**: Attach to an existing tmux session
- **`[zellij] session-name`**: Attach to an existing zellij session

## Related

- [Blog post (Japanese)](https://zenn.dev/because_and/articles/ghostty-tmux-zellij-claude-code-terminal-switcher) — Background and comparison of terminal environments for Claude Code

## License

MIT
