# Gradle install (manual) and basic setup

```bash
# Download Gradle (adjust version if needed)
wget https://services.gradle.org/distributions/gradle-7.4.1-bin.zip -P /tmp

# Prepare install directory
sudo mkdir -p /opt/gradle

# Extract
cd /tmp
sudo unzip -d /opt/gradle gradle-7.4.1-bin.zip
```

Optionally add a stable symlink and export PATH:
```bash
# Optional: create a symlink to the extracted directory (replace if different)
# ls /opt/gradle  # e.g., gradle-7.4.1
sudo ln -sfn /opt/gradle/gradle-7.4.1 /opt/gradle/latest

# Add to your shell profile (~/.bashrc or ~/.profile):
# export GRADLE_HOME=/opt/gradle/latest
# export PATH="$GRADLE_HOME/bin:$PATH"

# Reload the profile for current shell
# source ~/.bashrc

# Verify
# gradle -v
```
