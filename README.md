# Git & GitHub CLI (`gh`) Multi-Account Setup Guide

A practical guide for connecting local Git repositories to GitHub repositories when managing multiple GitHub accounts (e.g., a personal account `hejackson2` and a work account `work_account`).

---

## 1. Managing Multiple Accounts with GitHub CLI (`gh`)

The GitHub CLI (`gh`) supports multiple authenticated user accounts.

### Authenticate Both Accounts
Login to each account via `gh`:
```bash
# Login to personal account
gh auth login

# Login to work account
gh auth login
```
Follow the interactive prompts (select GitHub.com, SSH/HTTPS, upload SSH key, and authenticate).

### Check Active Accounts
To view all logged-in accounts and check which one is currently active:
```bash
gh auth status
```

### Switch Active Account
Switch the active account for CLI commands (e.g., `gh repo create`):
```bash
# Switch to personal account
gh auth switch --user hejackson2

# Switch to work account
gh auth switch --user work_account
```

---

## 2. Setting Up SSH Keys & `~/.ssh/config`

To allow Git to automatically select the correct SSH key for operations like `git push` and `git pull`, set up custom SSH host aliases in `~/.ssh/config`.

### Generate SSH Keys (if not already created)
```bash
# SSH key for personal account
ssh-keygen -t ed25519 -C "hejackson2@users.noreply.github.com" -f ~/.ssh/github_hejackson2_id_ed25519

# SSH key for work account
ssh-keygen -t ed25519 -C "work_account@example.com" -f ~/.ssh/github_work_account_id_ed25519
```
Add the public keys (`~/.ssh/github_<username>_id_ed25519.pub`) to your respective GitHub accounts under **Settings > SSH and GPG keys**.

### Configure `~/.ssh/config`
Add Host entries using distinct aliases for each account:

```config
# Personal GitHub Account (hejackson2)
Host github.com-hejackson2
  Hostname github.com
  User git
  IdentityFile ~/.ssh/github_hejackson2_id_ed25519
  IdentitiesOnly yes

# Work GitHub Account (work_account)
Host github.com-work_account
  Hostname github.com
  User git
  IdentityFile ~/.ssh/github_work_account_id_ed25519
  IdentitiesOnly yes
```

---

## 3. Creating & Linking a Local Repository

Follow these steps when creating a new local project and linking it to GitHub.

### Step 1: Initialize local repository
```bash
git init -b main
```

### Step 2: Configure local Git user identity
Set repo-specific Git credentials so commits use the correct identity:

**For personal project (`hejackson2`):**
```bash
git config --local user.name "hejackson2"
git config --local user.email "hejackson2@users.noreply.github.com"
# Optional: explicitly set SSH command
git config --local core.sshCommand "ssh -i ~/.ssh/github_hejackson2_id_ed25519"
```

**For work project (`work_account`):**
```bash
git config --local user.name "work_account"
git config --local user.email "work_account@example.com"
# Optional: explicitly set SSH command
git config --local core.sshCommand "ssh -i ~/.ssh/github_work_account_id_ed25519"
```

### Step 3: Switch `gh` to the matching account
Ensure `gh` is set to the desired account before creating the remote repository:
```bash
gh auth switch --user hejackson2
# or
gh auth switch --user work_account
```

### Step 4: Create remote repository on GitHub
Use `gh` to create the remote repo and push initial contents:
```bash
gh repo create <GH_REPO_NAME> --public --source=. --remote=origin --push
```

### Step 5: Configure SSH Remote URL to use host alias
To ensure Git uses the correct SSH key specified in `~/.ssh/config`, update the remote URL to use the custom host alias:

**For personal repository:**
```bash
git remote set-url origin git@github.com-hejackson2:hejackson2/<GH_REPO_NAME>.git
```

**For work repository:**
```bash
git remote set-url origin git@github.com-work_account:work_account/<GH_REPO_NAME>.git
```

> **Note:** The `github.com-<username>` host in the SSH URL matches the `Host github.com-<username>` entry defined in `~/.ssh/config`.

Verify the remote configuration:
```bash
git remote -v
```

---

## 4. Daily Workflow

Once configured, standard Git commands work seamlessly:

```bash
# Stage and commit changes
git add .
git commit -m "your commit message"

# Push changes to remote
git push origin main

# Pull updates from remote
git pull origin main
```
