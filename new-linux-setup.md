# New Linux Setup Guide

A step-by-step tutorial to set up a fresh Linux environment (tested on Debian/ChromeOS Linux) with essential developer tools.

---

## 1. Update the System

Always start with a full system update:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. Install Base Tools

```bash
sudo apt install -y git
```

---

## 3. Install VS Code

### Option A — Latest version (from Microsoft repo)

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm packages.microsoft.gpg
sudo apt update
sudo apt install -y code
```

### Option B — Older "copilot-bloatless" version (1.85.2, Nov 2023)

```bash
wget -O vscode-old.deb "https://update.code.visualstudio.com/1.85.2/linux-deb-x64/stable"
sudo apt install -y ./vscode-old.deb
rm vscode-old.deb
```

> **Important:** A popup will appear during install. Select **No**.  
> Saying Yes adds Microsoft’s repo, so the next `apt upgrade` will silently upgrade VS Code back to the latest version.

To use a different 2025 version, change the version number in the URL:

```
https://update.code.visualstudio.com/{VERSION}/linux-deb-x64/stable
```

Cleanup (optional):

```bash
rm ~/vscode-old.deb
```

---

## 4. Stop VS Code Auto-Updating

1. Open VS Code → Settings (`Ctrl+,`)
2. Search for `update mode`
3. Set **Update: Mode** to `none`

---

## 5. Enable "Open with VS Code" in Context Menu (Linux / Chromebook)

This older VS Code only registers itself for `.code-workspace` files. Expand it:

```bash
sudo vim /usr/share/applications/code.desktop
```

Find:

```
MimeType=application/x-code-workspace;
```

Replace with:

```
MimeType=text/plain;inode/directory;application/x-code-workspace;
```

Save and exit (`:wq`), then refresh:

```bash
sudo update-desktop-database
```

Restart the Chromebook once. After that, right-click in the Files app → **Open with → Code** should appear.

---

## 6. Install Node.js (LTS)

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

---

## 7. Install pnpm

```bash
curl -fsSL https://get.pnpm.io/install.sh | sh -
```

---

## 8. Install uv (Python package manager)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

---

## 9. Install Antigravity CLI

```bash
curl -fsSL https://antigravity.google/cli/install.sh | bash
```

---

## 10. Install OpenClaw

```bash
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon
```

Verify:

```bash
openclaw --version
openclaw doctor
openclaw gateway status
```

### PATH issue?

If `openclaw` is not found:

```bash
npm prefix -g
echo "$PATH"
```

Make sure the global bin path is in your `$PATH`. Add it to `.bashrc` / `.zshrc` if missing.

### Why `pnpm approve-builds`?

pnpm blocks package build/install scripts by default (security).  
`pnpm approve-builds -g` shows which global packages have pending scripts and lets you approve them one by one.

---

## 11. Final Steps

Add these to your `.bashrc` or `.zshrc`:

```bash
export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring
export NO_UPDATE_NOTIFIER=1
```

**Why these exports?**

- `PYTHON_KEYRING_BACKEND=...null.Keyring` — disables the system keyring. Without it, Python tools (uv, pip, etc.) try to use the desktop keyring on ChromeOS/Linux, which often fails or pops annoying password prompts.
- `NO_UPDATE_NOTIFIER=1` — stops npm/pnpm from showing constant “new version available” update notifications in the terminal.

---

## Uninstall / Cleanup

### Remove VS Code completely (and prevent apt from reinstalling the latest)

```bash
sudo apt remove --purge code -y
sudo rm /etc/apt/sources.list.d/vscode.list
```

---

## Manage OpenClaw Daemon

Temporarily stop (until reboot or manual start):

```bash
systemctl --user stop openclaw
```

Stop **and** prevent auto-start on boot:

```bash
systemctl --user stop openclaw
systemctl --user disable openclaw
```

Turn it back on later:

```bash
systemctl --user start openclaw
systemctl --user enable openclaw
```

Check status / resource usage:

```bash
systemctl --user status openclaw
```

---

## One-Go Setup Script (Copy & Paste)

Uses **VS Code 1.85.2 (Nov 2023)** and **permanently disables** the OpenClaw daemon.

```bash
#!/bin/bash
set -e

echo "=== Updating system ==="
sudo apt update && sudo apt upgrade -y

echo "=== Installing base tools ==="
sudo apt install -y git

echo "=== Installing VS Code 1.85.2 (Nov 2023) - copilot bloatless ==="
wget -O vscode-old.deb "https://update.code.visualstudio.com/1.85.2/linux-deb-x64/stable"
sudo apt install -y ./vscode-old.deb
rm vscode-old.deb
echo ">>> Select NO on the Microsoft repo popup if it appears <<<"

echo "=== Enabling 'Open with VS Code' in context menu ==="
sudo sed -i 's|MimeType=application/x-code-workspace;|MimeType=text/plain;inode/directory;application/x-code-workspace;|' /usr/share/applications/code.desktop
sudo update-desktop-database
echo ">>> Restart Chromebook once, then right-click in Files app → Open with → Code <<<"

echo "=== Installing Node.js ==="
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs

echo "=== Installing pnpm ==="
curl -fsSL https://get.pnpm.io/install.sh | sh -

echo "=== Installing uv (Python) ==="
curl -LsSf https://astral.sh/uv/install.sh | sh

echo "=== Installing Antigravity CLI ==="
curl -fsSL https://antigravity.google/cli/install.sh | bash

echo "=== Installing OpenClaw ==="
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon

echo "=== Permanently disabling OpenClaw daemon ==="
systemctl --user stop openclaw
systemctl --user disable openclaw

echo "=== Done ==="
echo ""
echo "========== MANUAL STEPS LEFT =========="
echo ""
echo "1. Stop VS Code auto-updating:"
echo "   - Open VS Code"
echo "   - Press Ctrl+,  (opens Settings)"
echo "   - Search for: update mode"
echo "   - Set 'Update: Mode' to: none"
echo ""
echo "2. Add these lines to .bashrc or .zshrc:"
echo "   export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring"
echo "   export NO_UPDATE_NOTIFIER=1"
echo ""
echo "3. Restart Chromebook once (for context menu fix to take effect)"
echo "========================================"
```
