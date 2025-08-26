# PostgreSQL install and basic setup (Ubuntu/Debian)

## Install PostgreSQL from the official PostgreSQL APT repository
```bash
# Prerequisites
sudo apt-get install -y wget ca-certificates

# Import the repository signing key
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Add the PostgreSQL APT repository (uses your distro codename)
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Update and install
sudo apt-get update
sudo apt-get install -y postgresql postgresql-contrib
```

## Change the default postgres user password
```bash
# Switch to the postgres system user and connect to the postgres DB
sudo -u postgres psql postgres

# Inside psql, run:
# \password postgres
# Enter new password when prompted
```

Notes:
- Use \q to quit psql.
- For advanced configuration (pg_hba.conf, postgresql.conf), consult official docs: https://www.postgresql.org/docs/
