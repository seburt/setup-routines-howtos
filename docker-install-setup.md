# Docker install and post-install (Ubuntu)

This follows the official docs: https://docs.docker.com/desktop/install/ubuntu/

## 1) Clean up previous/conflicting packages
```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

## 2) Add Docker’s official GPG key
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## 3) Add the repository to APT sources
```bash
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

## 4) Install Docker Engine
```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Optional: install a specific version
```bash
apt-cache madison docker-ce | awk '{ print $3 }'
# Example output: 5:24.0.0-1~ubuntu.22.04~jammy
VERSION_STRING=5:24.0.0-1~ubuntu.22.04~jammy
sudo apt-get install -y docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```

## 5) Verify installation
```bash
sudo docker run hello-world
```
If it fails, consult post-install doc: https://docs.docker.com/engine/install/linux-postinstall/

## 6) Post-install: manage Docker as a non-root user
```bash
# Create docker group (safe if it already exists)
sudo groupadd -f docker

# Add your user to the docker group
sudo usermod -aG docker $USER

# Apply new group without logout/login
newgrp docker

# Verify groups (docker should be listed)
groups

# If necessary, adjust permissions
sudo chown root:docker /var/run/docker.sock
sudo chown -R "$USER":"$USER" $HOME/.docker
sudo chmod -R g+rw "$HOME/.docker"
```