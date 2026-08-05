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

Restart your terminal, then reinstall VS Code extensions:

```bash
cat vscode-extensions.txt | xargs -L 1 code --install-extension
```

Add these to your `.bashrc` or `.zshrc`:

```bash
export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring
export NO_UPDATE_NOTIFIER=1
```

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
