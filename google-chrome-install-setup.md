# Google Chrome install and upgrade (Ubuntu/Debian)

## Install
```bash
# Download the latest stable .deb package
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

# Install it (handles dependencies)
sudo apt install ./google-chrome-stable_current_amd64.deb -y
```

## Upgrade
```bash
sudo apt-get update
sudo apt-get --only-upgrade install google-chrome-stable
```

Notes:
- The .deb installs the Google signing key and repository so future updates come via APT.
- If dpkg complains about missing dependencies, use: sudo apt -f install, then rerun the install.
