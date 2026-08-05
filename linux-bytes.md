# Basic File System Commands: Linux vs CMD vs PowerShell

## listing

Linux: `ls`  <br>

- `ls -al` shows all files in a directory with full details.

      Use cases:
      
      See hidden files (those starting with .)
      
      Check permissions, owners, sizes, timestamps
      
      Inspect directories in depth for debugging, config, deployment

CMD: `dir`  <br>
- dir /a /s > sizes.txt    
      This creates sizes.txt in the same folder with the full output (including hidden dirs).
PowerShell: `ls` / `dir` / `gci`  <br>

## show file content

Linux: `cat`  — dumps the whole file to screen at once  <br>
Linux: `less` — pager, view the file without loading it all into memory (scrollable, searchable)  <br>
CMD: `type`  <br>
PowerShell: `cat` / `gc`  <br>

### `less`

Pager for viewing files without loading the whole thing into memory.

**To Open:** `less file.txt`

**To Navigate:**
- `Space` or `f` — go forward one page
- `b` — go back one page
- `↓` or `j` — move down one line
- `↑` or `k` — move up one line
- `g` — jump to start of file
- `G` — jump to end of file
- `q` — close less

**To Search:**
- `/word` then Enter — finds "word" below your current position, moves you down to it
- `?word` then Enter — finds "word" above your current position, moves you up to it
- `n` — next match, same direction
- `N` — next match, opposite direction

**Other:**
- `less +F file` — opens in "follow mode" (like `tail -f`): instead of you scrolling, it auto-scrolls as new lines get appended to the file. Good for watching a live log update in real time. `Ctrl+C` to stop following (back to normal scrolling), `q` to quit.
  - `tail -f file.log` — shows the last few lines of a file, then keeps watching and prints new lines as they're added, live. `Ctrl+C` to stop.
- `command | less` — send another command's output into less to scroll it. `|` is a pipe: it connects one command's output to another command's input, instead of printing straight to the terminal.
  - `ls -l | less` — list files, scroll through them instead of a long dump
  - `history | less` — scroll through your command history
- `h` — shows all keyboard shortcuts inside less
- `~/.lesshst` — stores your search history so it's there next time; not important, skip backing it up

## create folder

Linux: `mkdir`  <br>
CMD: `md` / `mkdir`  <br>
PowerShell: `md` / `mkdir`  <br>

## delete file

Linux: `rm`  <br>
CMD: `del`  <br>
PowerShell: `rm`  <br>

## move/rename

Linux: `mv`  <br>
CMD: `move`  <br>
PowerShell: `mv` / `move`  <br>

## copy

Linux: `cp`  <br>
CMD: `copy`  <br>
PowerShell: `cp` / `copy` <br>

## remove folder

Linux: `rmdir`  <br>
CMD: `rd` / `rmdir`  <br>
PowerShell: `rmdir` / `rm -r`  <br>

## current directory

Linux: `pwd`   <br>
CMD: none (use `cd` alone)  <br>
PowerShell: `pwd`  <br>

## change directory

Linux: `cd` <br>
CMD: `cd`  <br>
PowerShell: `cd` <br>

## create empty file

Linux: `touch`   <br>
CMD: none (use `type nul > file`) <br>
PowerShell: `ni file`   <br>

<br>

# 𝔴𝔥𝔞𝔱'𝔰 𝔦𝔫𝔰𝔦𝔡𝔢

Command to check manually installed packages on Debian Bookworm (ChromeOS Linux container):

```
comm -23 <(apt-mark showmanual | sort) <(gzip -dc /var/log/installer/manifest.gz | awk '{print $1}' | sort)
```

> Nothing useless on my system. Everything in that list is core Debian stuff except the few tools I added myself on purpose.   

1. **Neofetch:**    
To quickly log system info, run: `neofetch`

2. **Bettercap:**   
Installed this while following a tutorial. I'll test it in the future. 
It's for network/security testing (MITM, Wi-Fi, packets, etc).

<br>

.android           → Android SDK/adb config & keys (for adb debugging)    
.app-store          → Chrome OS app store metadata (system-managed)    
.bash_history       → log of every command you typed in bash    
.bash_logout        → script that clears screen on terminal exit (privacy)    
.bashrc             → bash startup config — aliases, PATH, env vars, tool setup    
.cache              → temp cached data from apps (junk, regenerates)    
.config             → app configs (gh cli, various tools) — check before wiping    
.copilot            → GitHub Copilot CLI config/auth    
.dotnet             → .NET SDK install data    
.expo               → Expo CLI project cache/settings    
.gemini             → Gemini CLI config, MCP setup, conversation history    
.gitconfig          → your git identity (name/email) + git aliases    
.lesshst            → search history for the `less` pager tool    
.local              → user-local installed binaries/data (uv, pip, etc.)    
.npm                → npm package manager cache/logs    
.pki                → certificate/key store (system security)    
.profile            → login shell startup script — loads .bashrc, sets PATH    
.sommelierrc         → ChromeOS↔Linux display bridge config (unused/default)    
.sudo_as_admin_successful → empty marker file, just says "you've used sudo before"    
.viminfo            → Vim editor's command/search history + last cursor spots    
.vscode             → VSCode user settings/extensions config    
.vscode-shared      → VSCode shared config (Crostini-ChromeOS integration)    
.wget-hsts          → wget's memory of HTTPS-only sites (github.com etc.)    
.Xauthority         → binary auth token for X display server (session-only)    
.zshrc              → zsh startup config — aliases, PATH, env vars (same role as .bashrc)    

## Grouped by function, table style:

### 🕘 History/Memory files (tool remembers your usage)

| File | Tool | Remembers |
|------|------|----------|
| `.bash_history` | bash (terminal) | commands you typed |
| `.lesshst` | less (pager) | search terms used inside less |
| `.viminfo` | Vim (editor) | commands, searches, cursor positions |
| `.wget-hsts` | wget (downloader) | sites that require HTTPS-only |

### ⚙️ Shell startup/config files (run when terminal opens)

| File | Shell | Purpose |
|------|-------|---------|
| `.bashrc` | bash | aliases, PATH, env vars — runs on every new terminal |
| `.zshrc` | zsh | same as .bashrc but for zsh |
| `.profile` | login shell | runs once at login, loads .bashrc, sets PATH |
| `.bash_logout` | bash | runs on terminal exit — clears screen |

### 🔑 Identity/auth files

| File | Purpose |
|------|---------|
| `.gitconfig` | your git name/email + aliases |
| `.Xauthority` | session token to access the display (auto-regenerated) |
| `.sudo_as_admin_successful` | empty marker — "you've used sudo before" |

### 🛠️ Tool config/cache dirs

| Dir | Tool | Purpose |
|-----|------|---------|
| `.gemini` | Gemini CLI | MCP config, conversation history |
| `.copilot` | GitHub Copilot | CLI config/auth |
| `.vscode` / `.vscode-shared` | VSCode | editor settings, ChromeOS integration |
| `.npm` | npm | package cache/logs |
| `.expo` | Expo CLI | project cache/settings |
| `.config` | various | app configs (gh cli etc.) |

### 📦 System/junk (regenerates, never touch)

| Dir | Purpose |
|-----|---------|
| `.cache` | temp cached data |
| `.local` | user-installed binaries (uv, pip) |
| `.pki` | cert/key store |
| `.dotnet` | .NET SDK data |
| `.android` | adb/Android SDK config |
| `.app-store` | ChromeOS app store metadata |
| `.sommelierrc` | ChromeOS↔Linux display bridge config | 

All of `~/.local` is installed-tool infrastructure, not project data. Here's the breakdown:

**`.local/bin`**

| Item | Purpose |
|------|---------|
| `env` / `env.fish` | uv's PATH-setup script (sourced from .bashrc, remember?) |
| `sp`, `specifyplus` | symlinks to a uv-installed CLI tool called specifyplus |
| `uv`, `uvx` | the actual uv binaries |

**`.local/share`**

| Item | Purpose |
|------|---------|
| `applications` | desktop entry launchers (empty here) |
| `cros-motd` | ChromeOS "message of the day" file |
| `keyrings` | encrypted credential storage (used by keyring lib — but you disabled it with `PYTHON_KEYRING_BACKEND=null`) |
| `pnpm` | pnpm's package store |
| `recently-used.xbel` | recently opened files list (GTK apps) |
| `Trash` | Linux trash bin |
| `uv` | uv's internal data |
| `vscode-sqltools` | VSCode SQL extension data (remember the sqlite3 db work you did) |

**`.local/state`**

| Item | Purpose |
|------|---------|
| `aws-cloudformation-languageserver` | VSCode extension state, unused-looking |
| `pnpm` | pnpm runtime state |
| `wireplumber` | Linux audio session manager state |

# Quick APK Sideload Guide

1. **Move the APK** to the **Linux files** folder.
2. **Open Terminal** and connect:
`adb connect arc`
3. **Install the app**:
`adb -s arc install 'filename.apk'`

---

### Pro Tips

* **Update Apps:** To update an existing app without uninstalling, use:
`adb -s arc install -r 'filename.apk'`
* **Clean Up:** If you get a "device offline" error, run `adb kill-server` then start over from step 2.
