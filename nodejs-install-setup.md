# Node.js install and setup

This note covers several ways to install Node.js on Ubuntu/Debian, including NodeSource APT repo, manual tarball, and version managers (fnm, n).

## Option A: NodeSource APT repository (example: Node 17.x)
```bash
# Import the NodeSource GPG key
curl -sSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | sudo apt-key add -

# Create the list file
sudo nano /etc/apt/sources.list.d/nodesource.list
# Add the following lines for Ubuntu 20.04 (focal) and Node 17.x:
# deb https://deb.nodesource.com/node_17.x focal main
# deb-src https://deb.nodesource.com/node_17.x focal main

sudo apt update
sudo apt install -y nodejs
```

## Option B: Manual tarball + PATH update
```bash
mkdir ~/.node && cd "$_"
wget https://nodejs.org/dist/v18.14.0/node-v18.14.0-linux-x64.tar.xz
tar xf node-v18.14.0-linux-x64.tar.xz

# Add to ~/.profile (or ~/.bashrc):
# NodeJS
# export NODEJS_HOME=~/.node/node-v18.14.0-linux-x64/bin
# export PATH=$NODEJS_HOME:$PATH

# Reload the profile
. ~/.profile

# (Optional) Install npm from APT if missing
sudo apt install npm -y

# Remove APT node/npm if needed
sudo apt-get purge --autoremove nodejs npm
```

## Option C: Use fnm (Fast Node Manager)
```bash
# Install fnm using the official script (check the repo for the latest):
# https://github.com/Schniz/fnm/blob/master/.ci/install.sh

# Then install a specific node version
# fnm install <nodejs-version>
```

## Option D: Use `n` (Node.js version manager)
Reference: https://github.com/tj/n and https://askubuntu.com/questions/426750/how-can-i-update-my-nodejs-to-the-latest-version
```bash
# Install n globally via npm (requires npm)
npm install -g n

# Create cache folder (if missing) and set permissions
sudo mkdir -p /usr/local/n
sudo chown -R $(whoami) /usr/local/n

# Ensure required folders exist
sudo mkdir -p /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share
sudo chown -R $(whoami) /usr/local/bin /usr/local/lib /usr/local/include /usr/local/share

# Update Node.js to stable or latest
sudo npm cache clean -f
sudo npm install -g n
sudo n        # choose stable or latest interactively

# Fix /usr/bin/node if needed
sudo apt-get install --reinstall nodejs-legacy

# Undo example
sudo n rm 6.0.0     # replace with installed version
sudo npm uninstall -g n
```
