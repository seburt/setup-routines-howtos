# GitHub SSH setup

This guide shows how to generate SSH keys, configure the SSH client for GitHub, and use different SSH keys for multiple GitHub accounts.

## 1. Generate an SSH key

Use RSA 4096 (or ed25519 if preferred). Replace placeholders accordingly.

```bash
ssh-keygen -t rsa -b 4096 -C "email_here" -f ~/.ssh/key_name_here
```

## 2. Start ssh-agent and add your key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/key_name_here
```

## 3. (Optional) Create or edit SSH config

- System-wide SSH client config: `/etc/ssh/ssh_config` (readable by all users)
- User-specific SSH client config: `~/.ssh/config` (overrides the system config)

Example `~/.ssh/config` with two GitHub identities:

```sshconfig
# Default GitHub key (z0736190100)
Host github.com
    HostName github.com
    User z0736190100
    AddKeysToAgent yes
    IdentityFile ~/.ssh/z0736190100_git
    IdentitiesOnly yes

# Second GitHub key (seburt)
Host github-seburt
    HostName github.com
    User seburt
    AddKeysToAgent yes
    IdentityFile ~/.ssh/dell530_seburt_git
    IdentitiesOnly yes
```

Notes:
- `Host github.com` applies when you use URLs like `git@github.com:owner/repo.git`.
- `Host github-seburt` is an alias you can use in place of `github.com` to force a specific key.

## 4. Configure Git to use a specific key (when needed)

To force a repository to use a given key without changing the remote URL, set `core.sshCommand`:

```bash
git config core.sshCommand "ssh -i ~/.ssh/dell530_seburt_git -o IdentitiesOnly=yes"
```

Alternatively, clone or set the remote to use the alias from your SSH config (many IDEs like IntelliJ IDEA will pick this up automatically):

```text
git@github-seburt:seburt/{repo-url-here}.git
```

## 5. How it works

```text
Git command: git@github.com:seburt/repo.git
|
|  (Git reads core.sshCommand or ssh config)
v
SSH checks ~/.ssh/config:
|
|-- Host github-seburt
|      IdentityFile ~/.ssh/dell530_seburt_git
|
|-- Host github-z0736190100
       IdentityFile ~/.ssh/z0736190100_git

SSH chooses the matching Host alias or the specified key from core.sshCommand
        |
        v
Connects to github.com using the correct SSH key
```