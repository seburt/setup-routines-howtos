# Brave Browser install (Ubuntu/Debian)

Follow Brave’s official repository instructions.

```bash
# Prerequisites
sudo apt install -y apt-transport-https curl

# Add Brave’s GPG key to keyrings
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg \
  https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

# Add Brave repository
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=amd64] \
https://brave-browser-apt-release.s3.brave.com/ stable main" | \
sudo tee /etc/apt/sources.list.d/brave-browser-release.list

# Update and install
sudo apt update
sudo apt install -y brave-browser
```