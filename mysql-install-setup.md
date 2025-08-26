# MySQL install and setup

## Install MySQL Server
```bash
sudo apt-get update
sudo apt-get install mysql-server -y
```

## Make table names case-insensitive (lower_case_table_names=1)
Warning: This reinitializes the data directory (all data will be removed).
```bash
sudo service mysql stop
sudo rm -rf /var/lib/mysql                         # delete the MySQL data directory
sudo mkdir /var/lib/mysql                          # recreate data directory
sudo chown mysql:mysql /var/lib/mysql
sudo chmod 700 /var/lib/mysql
sudo mysqld --defaults-file=/etc/mysql/my.cnf \
  --initialize --lower_case_table_names=1 \
  --user=mysql --console                           # re-initialize MySQL
sudo service mysql start

# Retrieve the new temporary root password
sudo grep 'temporary password' /var/log/mysql/error.log

# Log in and set a new root password
sudo mysql -u root -p
# inside mysql:
# ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPa$$w0rd';
# SHOW VARIABLES LIKE 'lower_case_%';
```
Reference: https://dba.stackexchange.com/questions/59407/how-to-make-mysql-table-name-case-insensitive-in-ubuntu

## Enable General Query Log
```bash
# Find my.cnf
sudo find / -name my.cnf

# Edit main config
sudo nano /etc/mysql/my.cnf
# In the [mysqld] section, ensure the following are present/uncommented:
# general_log_file = /var/log/mysql/mysql.log
# general_log      = 1

# Restart MySQL
sudo systemctl restart mysql
# or: sudo service mysql restart

# Make the log readable (optional)
sudo chmod 777 /var/log/mysql/mysql.log

# Tail the log
tail -f /var/log/mysql/mysql.log
```