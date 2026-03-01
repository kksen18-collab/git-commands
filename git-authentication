# Git Authentication

## Two Ways to Authenticate

| Method | URL format | Identifies you via |
|---|---|---|
| SSH | `git@github.com:username/repo.git` | Private key |
| HTTPS + Credential Manager | `https://github.com/username/repo.git` | Username + token |

---

## Option 1 — SSH (Recommended)

Uses a **key pair** — private key stays on your machine, public key goes to GitHub. No passwords, no tokens, never expires.

### How it works
```
Your machine                    GitHub
private key  ←→  handshake  ←→  public key (uploaded to your account)
```

### Setup

```bash
# 1. generate SSH key
ssh-keygen -t ed25519 -C "your@email.com"
# saves to:
# ~/.ssh/id_ed25519      ← private key (never share this)
# ~/.ssh/id_ed25519.pub  ← public key (upload this to GitHub)

# 2. copy your public key
cat ~/.ssh/id_ed25519.pub

# 3. add to GitHub
# GitHub → Settings → SSH and GPG keys → New SSH key → paste it

# 4. test the connection
ssh -T git@github.com
# Hi username! You've successfully authenticated.

# 5. set your remote to use SSH
git remote set-url origin git@github.com:username/my-repo.git
```

### Why `git@github.com`?

```
git @ github.com : username/repo.git
 ↑       ↑
user    server
```

- `git` is GitHub's dedicated SSH user — everyone uses it
- GitHub identifies **who you are** by matching your public key, not the username in the URL
- Same as regular SSH: `ssh john@myserver.com`

### Pros and Cons

**Pros:**
- Never prompted for credentials
- Keys never expire
- Very secure — private key never leaves your machine

**Cons:**
- Need to generate a new key per machine
- Slight setup effort upfront

---

## Option 2 — HTTPS + Credential Manager

Uses your **GitHub username + personal access token (PAT)**. The credential manager saves them so you're only prompted once.

### How it works

```
git push
   ↓
Git checks credential manager → finds saved credentials
   ↓
Sends username + token to GitHub over HTTPS
   ↓
GitHub verifies token → allows push
```

### Setup

```bash
# 1. set the credential helper for your OS
git config --global credential.helper osxkeychain  # macOS
git config --global credential.helper manager      # Windows (Git Credential Manager)
git config --global credential.helper cache         # Linux (memory, 15 min)
git config --global credential.helper store         # Linux (plain text file)

# 2. set your remote to HTTPS
git remote set-url origin https://github.com/username/my-repo.git

# 3. push — Git will prompt you once
git push
# Username: your_username
# Password: your_personal_access_token  ← use a PAT, not your real password
```

After the first prompt, credentials are saved and you won't be asked again.

### What `git config --global credential.helper manager` does

It tells Git **which program to delegate credential storage to** — it doesn't store credentials itself, just registers the tool:

| Part | Meaning |
|---|---|
| `git config` | update a Git setting |
| `--global` | apply to all repos on your machine |
| `credential.helper` | setting that controls credential storage |
| `manager` | use Git Credential Manager as the tool |

### Where credentials are stored

| OS | Storage |
|---|---|
| macOS | Keychain Access app |
| Windows | Windows Credential Manager |
| Linux (cache) | Memory, expires after 15 min |
| Linux (store) | Plain text `~/.git-credentials` |

### Pros and Cons

**Pros:**
- Easy setup
- Works across machines (just re-enter credentials)

**Cons:**
- GitHub tokens expire — you'll need to update when they do
- `store` on Linux saves in plain text (less secure)

---

## Option 3 — Embed Token in URL (Avoid)

You can put credentials directly in the URL:

```bash
git remote set-url origin https://username:your_token@github.com/username/repo.git
```

**Why to avoid:**
- Token visible in plain text via `git remote -v`
- Risk of leaking in scripts or logs
- Use SSH or credential manager instead

---

## Comparison

| | SSH | Credential Manager | Token in URL |
|---|---|---|---|
| Security | ✅ Very secure | ✅ Secure (OS keychain) | ❌ Plain text risk |
| Setup effort | Medium | Easy | None |
| Token expiry | ❌ Never expires | ⚠️ Expires | ⚠️ Expires |
| Credential prompts | ❌ Never | ❌ After first setup | ❌ Never |
| Works across machines | ❌ Key per machine | ✅ Easy | ✅ Easy |
| Recommended | ✅ Yes | ✅ Yes | ❌ No |

---

## Quick Reference

```bash
# check current remote URL
git remote -v

# switch to SSH
git remote set-url origin git@github.com:username/repo.git

# switch to HTTPS
git remote set-url origin https://github.com/username/repo.git

# test SSH connection
ssh -T git@github.com

# set credential manager (macOS)
git config --global credential.helper osxkeychain

# set credential manager (Windows)
git config --global credential.helper manager
```

---

## Golden Rule

> Use **SSH** for day-to-day development — set it up once and never think about authentication again.
