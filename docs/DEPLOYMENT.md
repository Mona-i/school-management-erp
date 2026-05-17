# 🚀 SCHOOL MANAGEMENT ERP - PRODUCTION DEPLOYMENT GUIDE

## Complete Production Deployment Instructions

### Table of Contents
1. [VPS Deployment (Ubuntu 22.04)](#vps-deployment-ubuntu-2204)
2. [Docker Deployment](#docker-deployment)
3. [AWS Deployment](#aws-deployment)
4. [Monitoring & Maintenance](#monitoring--maintenance)
5. [Backup & Recovery](#backup--recovery)
6. [SSL/HTTPS Setup](#sslhttps-setup)
7. [Troubleshooting](#troubleshooting)

---

## VPS Deployment (Ubuntu 22.04)

### Phase 1: Server Setup (15 minutes)

#### Step 1.1: Initial Server Configuration

```bash
# SSH into server
ssh root@your_server_ip

# Update system
sudo apt update && sudo apt upgrade -y

# Set timezone
sudo timedatectl set-timezone Asia/Dubai

# Set hostname
sudo hostnamectl set-hostname school-erp

# Edit hosts file
sudo nano /etc/hosts
# Add: 127.0.0.1 school-erp.local
# Add: your_public_ip school-erp.com www.school-erp.com
```

#### Step 1.2: Create Deploy User

```bash
# Create non-root user
sudo useradd -m -s /bin/bash deployer
sudo usermod -aG sudo deployer

# Switch to deployer
sudo su - deployer

# Setup SSH key
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
# Paste your public key, then:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# Exit and configure sudo without password
exit
sudo nano /etc/sudoers
# Add: deployer ALL=(ALL) NOPASSWD:ALL
```

#### Step 1.3: Setup Firewall

```bash
# Enable firewall
sudo ufw enable

# Allow SSH
sudo ufw allow 22/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Check status
sudo ufw status
```

---

### Phase 2: Install Dependencies (20 minutes)

#### Step 2.1: Add PHP Repository

```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```

#### Step 2.2: Install PHP 8.3 & Extensions

```bash
sudo apt-get install -y \
    php8.3 \
    php8.3-fpm \
    php8.3-cli \
    php8.3-common \
    php8.3-mysql \
    php8.3-redis \
    php8.3-imagick \
    php8.3-gd \
    php8.3-curl \
    php8.3-xml \
    php8.3-zip \
    php8.3-bcmath \
    php8.3-json \
    php8.3-opcache \
    php8.3-dev

# Check version
php -v
```

#### Step 2.3: Configure PHP-FPM

```bash
# Edit configuration
sudo nano /etc/php/8.3/fpm/php.ini

# Update values:
memory_limit = 512M
upload_max_filesize = 100M
post_max_size = 100M
max_execution_time = 300
default_socket_timeout = 60

# Restart PHP-FPM
sudo systemctl restart php8.3-fpm
```

#### Step 2.4: Install MySQL 8.0

```bash
# Install MySQL
sudo apt-get install -y mysql-server-8.0

# Secure installation
sudo mysql_secure_installation

# Create database and user
sudo mysql -u root -p
```

```sql
CREATE DATABASE school_erp;
CREATE USER 'school_user'@'localhost' IDENTIFIED BY 'strong_password_here';
GRANT ALL PRIVILEGES ON school_erp.* TO 'school_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### Step 2.5: Install Redis

```bash
# Install Redis
sudo apt-get install -y redis-server

# Enable and start
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Test
redis-cli ping
# Output: PONG
```

#### Step 2.6: Install Nginx

```bash
# Install Nginx
sudo apt-get install -y nginx

# Enable and start
sudo systemctl enable nginx
sudo systemctl start nginx

# Check status
sudo systemctl status nginx
```

#### Step 2.7: Install Composer

```bash
# Download and install Composer
curl -sS https://getcomposer.org/installer -o composer-setup.php
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
rm composer-setup.php

# Verify
composer --version
```

#### Step 2.8: Install Node.js & npm

```bash
# Add Node.js repository
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js
sudo apt-get install -y nodejs

# Verify
node -v
npm -v
```

#### Step 2.9: Install Additional Tools

```bash
# Git
sudo apt-get install -y git

# Supervisor (for queue workers)
sudo apt-get install -y supervisor

# Certbot (for SSL)
sudo apt-get install -y certbot python3-certbot-nginx

# Monitoring
sudo apt-get install -y htop curl wget
```

---

### Phase 3: Deploy Application (25 minutes)

#### Step 3.1: Clone Repository

```bash
# Create web root
sudo mkdir -p /var/www/school-erp
sudo chown deployer:deployer /var/www/school-erp

# Clone repository
cd /var/www/school-erp
git clone https://github.com/Mona-i/school-management-erp.git .
```

#### Step 3.2: Install PHP Dependencies

```bash
cd /var/www/school-erp
composer install --optimize-autoloader --no-dev
```

#### Step 3.3: Install Node Dependencies

```bash
npm install
npm run build
```

#### Step 3.4: Configure Environment

```bash
cp .env.example .env
nano .env

# Update with production values:
APP_ENV=production
APP_DEBUG=false
DB_HOST=127.0.0.1
DB_DATABASE=school_erp
DB_USERNAME=school_user
DB_PASSWORD=your_strong_password
CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
```

#### Step 3.5: Generate Application Key

```bash
php artisan key:generate
```

#### Step 3.6: Set Directory Permissions

```bash
sudo chown -R www-data:www-data /var/www/school-erp
sudo chmod -R 755 /var/www/school-erp
sudo chmod -R 775 /var/www/school-erp/storage
sudo chmod -R 775 /var/www/school-erp/bootstrap/cache
```

#### Step 3.7: Run Database Migrations

```bash
php artisan migrate --force
php artisan db:seed --force
```

#### Step 3.8: Create Symbolic Links

```bash
php artisan storage:link
```

---

### Phase 4: Configure Nginx (12 minutes)

#### Step 4.1: Create Nginx Server Block

```bash
sudo nano /etc/nginx/sites-available/school-erp
```

**Add this configuration:**
```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    server_name school-erp.com www.school-erp.com;
    root /var/www/school-erp/public;
    index index.php index.html;

    client_max_body_size 100M;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;

    # Logs
    access_log /var/log/nginx/school-erp-access.log;
    error_log /var/log/nginx/school-erp-error.log;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    location ~ /\. {
        deny all;
    }

    gzip on;
    gzip_types text/plain text/css text/javascript application/json;
}
```

#### Step 4.2: Enable Server Block

```bash
# Create symbolic link
sudo ln -s /etc/nginx/sites-available/school-erp /etc/nginx/sites-enabled/school-erp

# Remove default
sudo rm -f /etc/nginx/sites-enabled/default

# Test Nginx configuration
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx
```

---

### Phase 5: Setup SSL Certificate (10 minutes)

#### Step 5.1: Install SSL Certificate

```bash
# Run Certbot
sudo certbot --nginx -d school-erp.com -d www.school-erp.com

# Follow prompts and agree to terms
```

#### Step 5.2: Auto-Renewal

```bash
# Enable timer
sudo systemctl enable certbot.timer
sudo systemctl start certbot.timer

# Test renewal
sudo certbot renew --dry-run
```

---

### Phase 6: Configure Supervisor for Queue Workers (12 minutes)

#### Step 6.1: Create Supervisor Configuration

```bash
sudo nano /etc/supervisor/conf.d/school-erp.conf
```

**Add configuration:**
```ini
[program:school-erp-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/school-erp/artisan queue:work redis --sleep=3 --tries=3 --timeout=90
autostart=true
autorestart=true
numprocs=4
redirect_stderr=true
stdout_logfile=/var/www/school-erp/storage/logs/worker.log
user=www-data

[program:school-erp-scheduler]
process_name=%(program_name)s
command=php /var/www/school-erp/artisan schedule:work
autostart=true
autorestart=true
user=www-data
redirect_stderr=true
stdout_logfile=/var/www/school-erp/storage/logs/scheduler.log
```

#### Step 6.2: Start Supervisor

```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start school-erp-worker:*
sudo supervisorctl start school-erp-scheduler
sudo supervisorctl status
```

---

### Phase 7: Setup Cron Jobs (8 minutes)

```bash
crontab -e

# Add this line:
* * * * * cd /var/www/school-erp && php artisan schedule:run >> /dev/null 2>&1

# Verify
crontab -l
```

---

### Phase 8: Automated Backups (10 minutes)

```bash
# Create backup directory
sudo mkdir -p /backups/school-erp
sudo chown deployer:deployer /backups/school-erp

# Create backup script
nano ~/backup-school-erp.sh
```

**Script content:**
```bash
#!/bin/bash

BACKUP_DIR="/backups/school-erp"
DATE=$(date +%Y%m%d_%H%M%S)
DB_NAME="school_erp"
DB_USER="school_user"
DB_PASS="your_password"

mkdir -p $BACKUP_DIR

# Database backup
mysqldump -u $DB_USER -p$DB_PASS $DB_NAME | gzip > $BACKUP_DIR/db_$DATE.sql.gz

# Files backup
tar --exclude='storage/logs' --exclude='node_modules' \
    -czf $BACKUP_DIR/files_$DATE.tar.gz /var/www/school-erp

# Delete old backups (keep last 30 days)
find $BACKUP_DIR -type f -mtime +30 -delete

echo "Backup completed: $DATE"
```

```bash
chmod +x ~/backup-school-erp.sh

# Schedule daily backup
(crontab -l 2>/dev/null; echo "0 2 * * * /home/deployer/backup-school-erp.sh") | crontab -
```

---

## Docker Deployment

### Using Docker Compose

```bash
# Clone and start
git clone https://github.com/Mona-i/school-management-erp.git
cd school-management-erp

docker-compose up -d

# Run migrations
docker-compose exec app php artisan migrate --seed

# Access at http://localhost
```

### Using Docker Swarm (Production)

```bash
# Initialize Swarm
docker swarm init

# Deploy stack
docker stack deploy -c docker-compose.yml school-erp

# Check services
docker service ls
```

---

## AWS Deployment

### Using Elastic Beanstalk

```bash
# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Configure credentials
aws configure

# Initialize EB
eb init -p "PHP 8.3" school-erp

# Create environment
eb create school-erp-prod

# Deploy
eb deploy

# Open in browser
eb open
```

---

## Monitoring & Maintenance

### View Application Logs

```bash
# Laravel logs
tail -f /var/www/school-erp/storage/logs/laravel.log

# Nginx logs
sudo tail -f /var/log/nginx/school-erp-error.log

# Queue worker logs
tail -f /var/www/school-erp/storage/logs/worker.log
```

### Monitor Resources

```bash
# Real-time monitoring
htop

# Disk usage
df -h

# Memory
free -h

# Database
sudo mysql -u root -e "SHOW PROCESSLIST;"
```

---

## Backup & Recovery

### Backup Database

```bash
mysqldump -u school_user -p school_erp | gzip > backup_$(date +%Y%m%d).sql.gz
```

### Restore Database

```bash
gunzip < backup_20240501.sql.gz | mysql -u school_user -p school_erp
```

---

## Troubleshooting

### 500 Internal Server Error

```bash
# Check Laravel logs
tail -f /var/www/school-erp/storage/logs/laravel.log

# Clear cache
php artisan cache:clear
php artisan view:clear
```

### Database Connection Error

```bash
# Test connection
mysql -u school_user -p school_erp -e "SELECT 1;"

# Check MySQL service
sudo systemctl status mysql
```

### Queue Jobs Not Processing

```bash
# Check Supervisor status
sudo supervisorctl status

# Restart workers
sudo supervisorctl restart school-erp-worker:*

# Check failed jobs
php artisan queue:failed
```

---

**Deployment Complete! 🚀**
