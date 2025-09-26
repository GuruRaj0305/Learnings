# tmux 

tmux is an open-source terminal multiplexer for Unix-like operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window.

## installation 

```bash
sudo apt install tmux -y
```

## to start 

`tmux` -> it starts and shows `[0] 0:bash*`
+ [0] -> indicates current session number
+ 0:bash -> in window 0
+ `*` -> indicates this is your active window 

 ## usefull cmds

 + `ctrl + b, shift + %` -> to create new terminal vertically
 + `ctrl + b, shift + <any arrow>` ->  to shitch between terminals.
 + `ctrl + b, shift + "` -> to create new terminal horizontally

# TMUX Shortcuts & Commands

## Basic Commands

| Command | Description |
|---------|-------------|
| `tmux` | Start a new tmux session |
| `tmux new -s session_name` | Start a new session with a custom name |
| `Ctrl+b d` | Detach from the current session |
| `tmux ls` | List all existing sessions |
| `tmux attach -t session_name` | Attach to a session |
| `tmux kill-session -t session_name` | Kill a specific session |
| `tmux kill-server` | Kill all tmux sessions |

---

## Windows & Panes

### Window Management

| Shortcut | Description |
|----------|-------------|
| `Ctrl+b c` | Create a new window |
| `Ctrl+b w` | List all windows |
| `Ctrl+b n` | Move to the next window |
| `Ctrl+b p` | Move to the previous window |
| `Ctrl+b ,` | Rename the current window |
| `Ctrl+b &` | Kill the current window |

### Pane Management

| Shortcut | Description |
|----------|-------------|
| `Ctrl+b %` | Split pane **vertically** |
| `Ctrl+b "` | Split pane **horizontally** |
| `Ctrl+b o` | Switch to the next pane |
| `Ctrl+b ;` | Switch to the last active pane |
| `Ctrl+b x` | Close the current pane |
| `Ctrl+b {` | Move current pane **left/up** |
| `Ctrl+b }` | Move current pane **right/down** |
| `Ctrl+b z` | Toggle pane **zoom** (fullscreen) |

---

## Session Management Tips

- **Detach & reattach sessions**  
```bash
tmux detach   # inside tmux
tmux attach   # reconnect later
```