# Apache Maven install (manual) and environment setup

```bash
# Download Maven (adjust version if needed)
wget -P ~/Downloads/ https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz

# Extract to /usr/local
sudo tar -xf ~/Downloads/apache-maven-3.9.9-bin.tar.gz -C /usr/local

# Set ownership and permissions
sudo chown -R root:root /usr/local/apache-maven-3.9.9
sudo chmod 755 /usr/local/apache-maven-3.9.9

# Create a stable symlink
sudo ln -s /usr/local/apache-maven-3.9.9 /usr/local/maven

# Add environment variables (append to ~/.bashrc or ~/.profile)
# echo 'export MAVEN_HOME=/usr/local/maven' | tee -a ~/.bashrc
# echo 'export PATH="${PATH}:${MAVEN_HOME}/bin"' | tee -a ~/.bashrc

# For current shell session, you can also export directly:
# export MAVEN_HOME=/usr/local/maven
# export PATH="${PATH}:${MAVEN_HOME}/bin"
```

Notes:
- After editing ~/.bashrc or ~/.profile, reload with: source ~/.bashrc (or log out/in).
- Verify installation: mvn -v
