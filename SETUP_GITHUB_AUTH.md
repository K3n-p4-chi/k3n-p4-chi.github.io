# GitHub Authentication Setup

## Current Issue
Git is trying to use wrong credentials (xjdot87) instead of your username (k3n-p4-chi).

## Solution: Personal Access Token

### Step 1: Create Token on GitHub

1. Go to: https://github.com/settings/tokens
2. Click **"Generate new token (classic)"**
3. Fill in:
   - **Note:** "Blog Publishing from VSCode"
   - **Expiration:** 90 days (or No expiration)
   - **Select scopes:**
     - ✅ **repo** (check this - gives full control of repositories)
4. Click **"Generate token"**
5. **COPY THE TOKEN** - you'll need it in next step
   - It looks like: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

### Step 2: Update Git Credentials

**Option A: Let Git Prompt You (Easiest)**

Run this command - Git will prompt for username and password:
```bash
cd "C:\Users\frenc\My Drive\k3n-p4-chi-blog"
git push
```

When prompted:
- **Username:** `k3n-p4-chi`
- **Password:** [paste your Personal Access Token]

Windows Credential Manager will save this for future use.

**Option B: Update Credential Manager Directly**

1. Press **Windows Key** + type "Credential Manager"
2. Click **"Windows Credentials"**
3. Find entry for `git:https://github.com`
4. Click **"Edit"**
5. Update:
   - **Username:** `k3n-p4-chi`
   - **Password:** [your Personal Access Token]
6. Click **"Save"**

**Option C: Command Line (if credential manager entry doesn't exist)**

```bash
# Remove old credential
git credential-manager delete https://github.com

# Next push will prompt for new credentials
cd "C:\Users\frenc\My Drive\k3n-p4-chi-blog"
git push
```

### Step 3: Test

```bash
cd "C:\Users\frenc\My Drive\k3n-p4-chi-blog"
git push
```

Should see:
```
Enumerating objects: X, done.
Counting objects: 100% (X/X), done.
...
To https://github.com/k3n-p4-chi/k3n-p4-chi.github.io.git
   abc1234..def5678  main -> main
```

### Step 4: Verify Blog Updated

Wait ~1 minute, then check:
https://k3n-p4-chi.github.io/planning-tools/

You should see your planning session post!

---

## Troubleshooting

### Still shows xjdot87?
Clear the credential completely:
```bash
git config --global --unset credential.helper
git config --global credential.helper manager
```

Then try push again - it will prompt for credentials.

### Token not working?
- Make sure you copied the ENTIRE token
- Verify the token has **repo** scope enabled
- Token may have expired - generate a new one

### Alternative: Use SSH Instead
If tokens are problematic, you can switch to SSH keys:
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "jdd.pougoue@gmail.com"

# Add to GitHub: Settings → SSH Keys

# Update remote URL
cd "C:\Users\frenc\My Drive\k3n-p4-chi-blog"
git remote set-url origin git@github.com:k3n-p4-chi/k3n-p4-chi.github.io.git
```

---

## Quick Reference

**Your GitHub username:** `k3n-p4-chi`
**Your repository:** `k3n-p4-chi.github.io`
**Blog URL:** https://k3n-p4-chi.github.io
**Current branch:** `main`
**Commits ready to push:** 1 (planning session post)
