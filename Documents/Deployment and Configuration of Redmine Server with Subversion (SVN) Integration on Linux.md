# Deployment and Configuration of Redmine Server with Subversion (SVN) Integration on Linux
### Introduction:
This project involves the installation, deployment, and configuration of the Redmine Project Management Tool integrated with Subversion (SVN) version control system on a Linux-based server.

**Purpose:**
The goal was to enable project tracking, issue management, and seamless integration with a centralized version control system to enhance collaboration and productivity.

**Objectives:**

Deploy Redmine on a Linux server.

Configure required dependencies (**Ruby**, **Rails**, **Passenger**, **MySQL/MariaDB**, **Apache/Nginx**).

Install and configure **Subversion (SVN)** repository.

Integrate **Redmine** with **SVN** for version control.

Ensure secure access via user authentication and permissions.

Test and validate the integration for smooth project management.

#### Install Apache:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl status apache2
```
#### Install MariaDB:
```bash
sudo apt update
sudo apt install mariadb-server
sudo systemctl start mariadb
sudo systemctl status mariadb
sudo mariadb
```
```bash
sudo mariadb
```
```bash
Welcome to the MariaDB monitor.  Commands end with ; or g.
Your MariaDB connection id is 32
Server version: 10.11.2-MariaDB-1 Ubuntu 23.04
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or 'h' for help. Type 'c' to clear the current input statement.
MariaDB [(none)]> 
```
#### Create a Redmine Database:
```bash
CREATE DATABASE redminedb CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER redminedbuser@localhost IDENTIFIED BY '12345678';
GRANT ALL ON redminedb.* TO redminedbuser@localhost WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```
#### Install Ruby and Ruby development package
```bash
sudo apt install ruby ruby-dev -y
```
#### Install Readline, Zlib, and SSL libraries
```bash
sudo apt install -y libreadline-dev zlib1g-dev libssl-dev
```
#### Install build tools
```bash
sudo apt install build-essential -y
```
#### Install cURL and MySQL client libraries
```bash 
sudo apt install -y libcurl4-openssl-dev libmysqlclient-dev
```
#### Install PostgreSQL and APR Util libraries
```bash
sudo apt install libpq-dev libaprutil1-dev -y
```
#### Install Apache APR and development headers

```bash
sudo apt install libapr1-dev apache2-dev
```
#### Install RVM Dependencies (with Required Environment Dependencies):
```bash
sudo apt install -y gnupg2 curl
```
#### Install RVM (Ruby Version Manager)

*Download and install RVM (Stable version)*

```bash
curl -sSL https://get.rvm.io | bash -s stable
```
#### Load RVM into current shell session
```bash
source ~/.rvm/scripts/rvm
```
#### Import RVM GPG Keys (Security Verification)
```bash
# Import GPG key by mpapis (RVM maintainer)
curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
```
#### Import GPG key by pkuczynski (another RVM maintainer)
```bash
# GPG public key of Piotr Kuczynsk
curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -
```
#### Install Latest Stable Version of Ruby 3.3 (via RVM)
```bash
# Install OpenSSL (through RVM)
rvm pkg install openssl
# Update RVM to the latest version
rvm get head
# Install Ruby 3.3 LTS Stable Version
rvm install ruby-3.3
# Set Ruby 3.3 as the default version
rvm use ruby-3.3 --default
```
#### Install Redmine 6.0.6 Version
```bash
cd /tmp
wget https://www.redmine.org/releases/redmine-6.0.6.tar.gz

# Extract the Redmine package
tar -xvzf redmine-6.0.6.tar.gz

sudo -u www-data cp -r redmine-6.0.3/* /var/lib/redmine

# by default database configuration ‘config/database.yml.example‘ to ‘config/database.yml‘. 

sudo -u www-data cp /var/lib/redmine/config/database.yml.example /var/lib/redmine/config/database.yml

```
#### Integrating Database with the Projct of Redmine
```bash
sudo -u www-data nano /var/lib/redmine/config/database.yml

# in the ‘production‘ section, change the details of the MySQL database and user.

  database: redminedb
  host: localhost
  username: redminedbuser
  password: "12345678"

# Save the file and exit 
```
#### Install Bundler (Ruby Dependency Manager)
```bash
# Install Bundler using gem
sudo gem install bundler
```
#### Assign Ownership & Permission to Redmine
```bash
# Redmine by default directory path */var/lib/redmine*
sudo mkdir /var/lib/redmine
sudo chown -R www-data:www-data /var/lib/redmine
```
#### Install Ruby Dependencies for Redmine
```bash

# Change to the Redmine directory 
cd /var/lib/redmine

# Configure Bundler to exclude development & test dependencies
sudo bundle config set --local without 'development test'
# Add & Replace this line with the existing line in the file. 

# Install all required gems using Bundler
sudo bundle install
```

#### Install Ruby Dependencies for Redmine
```bash

# Change to the Redmine directory 
cd /var/lib/redmine

# Configure Bundler to exclude development & test dependencies
sudo bundle config set --local without 'development test'
# Add & Replace this line with the existing line in the file. 

# Install all required gems using Bundler
sudo bundle install
```
#### Configure Redmine Setup
```bash
# Generate a secret token
sudo -u www-data bin/rake generate_secret_token

# Run database migrations
sudo -u www-data bin/rake db:migrate RAILS_ENV="production"
```
#### Load Redmine Default Data
```bash
# Insert default roles, trackers, and workflows
sudo RAILS_ENV=production bundle exec rake redmine:load_default_data
```
#### Install  Phusion Passenger

```bash
sudo gem install passenger
sudo passenger-install-apache2-module --auto --languages ruby
sudo passenger-install-apache2-module --snippet
```
#### Apache Configuration for Redmine

```bash
sudo nano /etc/apache2/sites-available/redmine.conf
```
```bash
<VirtualHost *:80>
  ServerName redmine.example.com
  ServerAdmin admin@example.com
  DocumentRoot /var/lib/redmine/public

<Directory "/var/lib/redmine/public">
  Require all granted
</Directory>

LoadModule passenger_module /var/lib/gems/3.2.0/gems/passenger-6.0.25/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
  PassengerRoot /var/lib/gems/3.2.0/gems/passenger-6.0.27
  PassengerDefaultRuby /usr/bin/ruby3.2
</IfModule>

# Allow access to Redmine's installation directory
<Directory /var/lib/redmine/public>
    Allow from all
    Options -MultiViews
    Require all granted
</Directory>
</VirtualHost>
```
```bash
sudo systemctl restart apache2
```
