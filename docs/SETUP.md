# 🚀 LOCAL DEVELOPMENT SETUP GUIDE

## Complete Step-by-Step Installation for School Management ERP

---

## **Part 1: Prerequisites Installation**

### **For macOS (Intel & Apple Silicon)**

```bash
# 1. Install Homebrew (if not installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Install PHP 8.3
brew tap shivammathur/php
brew install shivammathur/php/php@8.3
brew link php@8.3

# 3. Verify PHP installation
php -v
# Output: PHP 8.3.x

# 4. Install MySQL 8.0
brew install mysql@8.0
brew services start mysql@8.0

# 5. Install Redis
brew install redis
brew services start redis

# 6. Install Node.js (v20)
brew install node@20
brew link node@20

# 7. Verify installations
node -v   # v20.x.x
npm -v    # 10.x.x
mysql --version
redis-cli ping  # PONG

# 8. Install Composer
curl -sS https://getcomposer.org/installer -o composer-setup.php
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
rm composer-setup.php

# 9. Verify Composer
composer --version
```

### **For Windows (with WSL2)**

```bash
# 1. Install WSL2
# Go to Microsoft Store → Download Ubuntu 22.04

# 2. Open Ubuntu terminal and run:
sudo apt update && sudo apt upgrade -y

# 3. Install PHP 8.3
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.3 php8.3-cli php8.3-fpm php8.3-mysql \
    php8.3-redis php8.3-gd php8.3-curl php8.3-xml php8.3-zip

# 4. Install MySQL
sudo apt install mysql-server

# 5. Install Redis
sudo apt install redis-server

# 6. Install Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# 7. Install Composer
curl -sS https://getcomposer.org/installer -o composer-setup.php
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

### **For Ubuntu/Linux**

```bash
# 1. Update system
sudo apt update && sudo apt upgrade -y

# 2. Install PHP 8.3
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.3 php8.3-cli php8.3-fpm php8.3-mysql \
    php8.3-redis php8.3-imagick php8.3-gd php8.3-curl \
    php8.3-xml php8.3-zip php8.3-bcmath

# 3. Install MySQL
sudo apt install mysql-server

# 4. Install Redis
sudo apt install redis-server

# 5. Install Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install nodejs

# 6. Install Composer
curl -sS https://getcomposer.org/installer -o composer-setup.php
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

---

## **Part 2: Database Setup**

### **Create Database & User**

```bash
# 1. Connect to MySQL
mysql -u root -p

# 2. Run these commands in MySQL prompt
CREATE DATABASE school_erp;
CREATE USER 'school_user'@'localhost' IDENTIFIED BY 'strong_password_123';
GRANT ALL PRIVILEGES ON school_erp.* TO 'school_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

# 3. Verify connection
mysql -u school_user -p school_erp -e "SELECT 1;"
```

### **Redis Verification**

```bash
# 1. Start Redis
redis-server  # macOS
# or
sudo systemctl start redis-server  # Linux

# 2. Test connection
redis-cli ping
# Output: PONG

# 3. Check info
redis-cli INFO
```

---

## **Part 3: Clone & Install Project**

### **Step 1: Clone Repository**

```bash
# Using HTTPS
git clone https://github.com/Mona-i/school-management-erp.git

# Or using SSH (if SSH key configured)
git clone git@github.com:Mona-i/school-management-erp.git

# Navigate to project
cd school-management-erp
```

### **Step 2: Install PHP Dependencies**

```bash
# Install Composer packages
composer install

# If you get memory issues:
php -d memory_limit=-1 composer install

# Update dependencies (optional)
composer update
```

### **Step 3: Install Node Dependencies**

```bash
# Install npm packages
npm install

# Or using yarn (if preferred)
yarn install

# Build assets
npm run build

# For development with hot reload
npm run dev  # Run in separate terminal
```

### **Step 4: Environment Configuration**

```bash
# Copy environment file
cp .env.example .env

# Edit .env file
nano .env  # or use your preferred editor
```

**Important .env values to update:**

```env
APP_NAME="School Management ERP"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=school_erp
DB_USERNAME=school_user
DB_PASSWORD=strong_password_123

# Cache & Session
CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# Mail (use Mailtrap for testing)
MAIL_MAILER=smtp
MAIL_HOST=sandbox.smtp.mailtrap.io
MAIL_PORT=465
MAIL_USERNAME=your_mailtrap_username
MAIL_PASSWORD=your_mailtrap_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@school-erp.local
MAIL_FROM_NAME="School ERP"

# Filesystem
FILESYSTEM_DISK=public

# Optional: Payment Gateways (for testing)
STRIPE_PUBLIC_KEY=pk_test_your_key
STRIPE_SECRET_KEY=sk_test_your_key
```

### **Step 5: Generate Application Key**

```bash
php artisan key:generate

# Verify (should show key in .env)
grep APP_KEY .env
```

### **Step 6: Database Migrations & Seeding**

```bash
# Run all migrations
php artisan migrate

# Seed database with demo data
php artisan db:seed

# Or run specific seeder
php artisan db:seed --class=RolePermissionSeeder
php artisan db:seed --class=SchoolSeeder
php artisan db:seed --class=UserSeeder
php artisan db:seed --class=ClassSeeder

# Verify database
mysql -u school_user -p school_erp -e "SHOW TABLES;"
```

### **Step 7: Create Storage Link**

```bash
# Create symlink for file uploads
php artisan storage:link

# Verify
ls -la storage/app/public
```

### **Step 8: Install Spatie Permissions**

```bash
# Publish migrations
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"

# Run migrations
php artisan migrate

# Seed permissions
php artisan db:seed --class=RolePermissionSeeder
```

---

## **Part 4: Start Development Servers**

### **Terminal 1: Laravel Development Server**

```bash
cd school-management-erp
php artisan serve
# Output: Server running on http://127.0.0.1:8000
```

### **Terminal 2: Vite Development Server (Asset Compilation)**

```bash
cd school-management-erp
npm run dev
# Hot reload enabled for changes
```

### **Terminal 3: Queue Worker (Background Jobs)**

```bash
cd school-management-erp
php artisan queue:work redis --sleep=3 --tries=3
# Processes jobs in background
```

### **Terminal 4: Scheduler (Optional - for scheduled tasks)**

```bash
cd school-management-erp
php artisan schedule:work
# Runs scheduled commands
```

---

## **Part 5: Access Application**

### **URL & Credentials**

- **Application URL:** http://localhost:8000
- **Admin Email:** admin@school.test
- **Password:** password

### **Other User Credentials**

| Role | Email | Password |
|------|-------|----------|
| Teacher | teacher@school.test | password |
| Student | student@school.test | password |
| Parent | parent@school.test | password |
| Accountant | accountant@school.test | password |
| Librarian | librarian@school.test | password |
| HR Manager | hr@school.test | password |

### **Admin Tools**

- **PhpMyAdmin:** n/a (use MySQL client)
- **Laravel Debugbar:** Visible at bottom of pages
- **Telescope:** http://localhost:8000/telescope (optional)
- **Logs:** `storage/logs/laravel.log`

---

## **Part 6: Optional Development Tools**

### **Install Telescope (Debugging)**

```bash
composer require laravel/telescope --dev

php artisan telescope:install

# Access at http://localhost:8000/telescope
```

### **Install Tinker (REPL)**

```bash
# Already included, but can be tested:
php artisan tinker

# Then in tinker:
>>> User::count()
>>> Student::where('status', 'active')->count()
```

### **Install IDE Helper (For IDE autocompletion)**

```bash
composer require --dev barryvdh/laravel-ide-helper

php artisan ide-helper:generate
php artisan ide-helper:models --nowrite
```

---

## **Part 7: Useful Development Commands**

### **Database Commands**

```bash
# Migrate from scratch
php artisan migrate:fresh --seed

# Rollback last migration batch
php artisan migrate:rollback

# Rollback all and re-run
php artisan migrate:refresh

# Check migration status
php artisan migrate:status

# Create migration
php artisan make:migration create_table_name
```

### **Cache Commands**

```bash
# Clear all cache
php artisan cache:clear
php artisan cache:flush

# Clear specific cache
php artisan cache:forget key_name

# Clear config cache
php artisan config:clear

# Clear view cache
php artisan view:clear

# Clear route cache
php artisan route:clear
```

### **Queue Commands**

```bash
# Process queue jobs
php artisan queue:work

# Failed jobs
php artisan queue:failed
php artisan queue:failed-table
php artisan queue:retry all

# Clear failed jobs
php artisan queue:flush
```

### **Model/Migration Generation**

```bash
# Generate model with migration, factory, seeder
php artisan make:model Student -mfs

# Controller with resource methods
php artisan make:controller StudentController --resource

# Form Request
php artisan make:request StoreStudentRequest

# Service class
php artisan make:class Services/StudentService
```

---

## **Part 8: Troubleshooting**

### **Issue: Connection refused**

```bash
# Check MySQL is running
mysql -u root -e "SELECT 1;"

# If not running:
brew services start mysql@8.0  # macOS
sudo systemctl start mysql      # Linux

# Check Redis is running
redis-cli ping
# Should return PONG
```

### **Issue: Permission denied (storage)**

```bash
# Fix storage permissions
chmod -R 775 storage
chmod -R 775 bootstrap/cache
chown -R $USER:$USER storage bootstrap/cache
```

### **Issue: Port already in use**

```bash
# Laravel Serve on different port
php artisan serve --port=8001

# Find and kill process on port 8000
lsof -ti:8000 | xargs kill -9

# npm run dev on different port
npm run dev -- --port 5174
```

### **Issue: Composer memory error**

```bash
# Increase PHP memory limit
php -d memory_limit=-1 composer install

# Or permanently in php.ini:
memory_limit = 512M
```

### **Issue: npm install fails**

```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

---

## **Part 9: Testing the Application**

### **Run Tests**

```bash
# Run all tests
php artisan test

# Run specific test file
php artisan test tests/Feature/Student/StudentAdmissionTest.php

# Run with coverage
php artisan test --coverage

# Run tests in parallel
php artisan test --parallel
```

### **Manual Testing Workflow**

1. **Login:** http://localhost:8000/login
2. **Create Student:** Students → Create New
3. **Mark Attendance:** Attendance → Mark Attendance
4. **Create Exam:** Exams → Create New
5. **View Reports:** Reports → Student Report

---

## **Part 10: Git Setup for Development**

### **Initial Commit**

```bash
# Configure git
git config user.name "Your Name"
git config user.email "your.email@example.com"

# Check current branch
git branch

# Create feature branch
git checkout -b feature/student-management

# Make changes, then commit
git add .
git commit -m "feat: add student management module"

# Push to remote
git push origin feature/student-management
```

### **Useful Git Commands**

```bash
# Check status
git status

# View changes
git diff

# Unstage changes
git reset HEAD file_name

# Discard changes
git checkout -- file_name

# View commit history
git log --oneline

# Sync with remote
git pull origin main
git fetch --all
```

---

## **Part 11: IDE Configuration**

### **VS Code Settings (.vscode/settings.json)**

```json
{
    "php.version": "8.3",
    "[php]": {
        "editor.defaultFormatter": "bmewburn.vscode-intelephense-client",
        "editor.formatOnSave": true
    },
    "intelephense.format.enable": true,
    "intelephense.format.rules": {
        "arrayBracketSpacing": "none",
        "objectBracketSpacing": "none"
    },
    "[blade]": {
        "editor.defaultFormatter": "shufo.vscode-blade-formatter"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    }
}
```

### **PHP IntelliSense**

```bash
# Install IDE Helper
composer require --dev barryvdh/laravel-ide-helper

# Generate helpers
php artisan ide-helper:generate
php artisan ide-helper:models --nowrite
```

---

## **Part 12: Performance Tips for Development**

```bash
# 1. Enable query logging (in routes/web.php)
use Illuminate\Support\Facades\DB;
if (config('app.debug')) {
    DB::listen(function ($query) {
        logger()->debug($query->sql, $query->bindings);
    });
}

# 2. Use eager loading to prevent N+1 queries
Student::with('class', 'section', 'guardians')->get();

# 3. Use Redis for session storage (already configured)

# 4. Cache frequently accessed data
Cache::remember('classes', now()->addHours(24), fn() => Class::all());

# 5. Use queues for heavy operations
dispatch(new SendNotifications($students));
```

---

## **Quick Reference Cheatsheet**

```bash
# Setup from scratch (5 minutes)
git clone https://github.com/Mona-i/school-management-erp.git
cd school-management-erp
cp .env.example .env
composer install
npm install
php artisan key:generate
php artisan migrate --seed
npm run build
php artisan serve

# Fresh database
php artisan migrate:fresh --seed

# Run tests
php artisan test

# Clear everything
php artisan cache:clear && php artisan config:clear && php artisan view:clear

# Create new feature
php artisan make:model Feature -mfs
php artisan make:controller FeatureController --resource
php artisan make:request StoreFeatureRequest
```

---

**Your local development environment is now ready! 🎉**

Start with the video guide Part 2 for detailed walkthrough.
