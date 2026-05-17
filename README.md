# 🎓 School Management ERP System

## Production-Ready Laravel 12 Enterprise Solution

A comprehensive, scalable School Management Enterprise Resource Planning (ERP) system built with Laravel 12, PHP 8.3+, MySQL, and modern web technologies.

### ✨ Features

#### 1. **Authentication & Authorization**
- Multi-role access control (8 roles)
- Two-factor authentication (2FA)
- Email verification
- Password reset & recovery
- Session management
- API token authentication (Sanctum)
- Role-based access control (RBAC)
- Permission management

#### 2. **Student Management**
- Complete student admission system
- Student profiles with photo uploads
- Guardian/parent information management
- Medical records tracking
- Document management
- Bulk import/export (Excel/CSV)
- Student ID card generation (PDF)
- Advanced search & filtering
- Student promotion system

#### 3. **Teacher Management**
- Teacher profiles & credentials
- Subject assignments
- Class assignments
- Teaching load management
- Performance tracking
- Leave management

#### 4. **Academic Management**
- Class & section management
- Subject management
- Academic session handling
- Timetable scheduling
- Assignment management
- Homework submission & tracking

#### 5. **Attendance System**
- Daily attendance marking
- Bulk attendance upload (Excel)
- QR code scanning
- Mobile attendance app support
- Attendance analytics & reports
- SMS/Email notifications for absence
- Attendance percentage calculation

#### 6. **Exam & Grading System**
- Exam scheduling & management
- Mark entry & calculation
- Automatic grade calculation (A-F)
- GPA computation
- Report card generation & printing
- Result publishing to students
- Grade analytics & statistics
- Comparative analysis

#### 7. **Finance & Payments**
- Fee structure management by class
- Automatic invoice generation
- Online payment processing with:
  - Stripe integration
  - Paystack integration
  - Flutterwave integration
- Bank transfer tracking
- Cash payment recording
- Expense tracking & approval
- Payroll management
- Financial reports & analytics
- Outstanding payment tracking

#### 8. **Library Management**
- Book inventory management
- ISBN & barcode support
- Borrowing/returning tracking
- Fine calculation for overdue books
- Due date reminders
- Overdue tracking
- Book availability status
- Member borrowing history

#### 9. **Hostel Management**
- Room allocation
- Occupancy tracking
- Hostel fee management
- Maintenance tracking
- Visitor log
- Leave tracking

#### 10. **Transport Management**
- Vehicle records & maintenance
- Route management
- Driver information & verification
- Student allocation to routes
- Transport fee tracking

#### 11. **HR Management**
- Staff management & records
- Leave request handling
- Leave approval workflow
- Performance tracking
- Payroll calculation & management
- Salary slip generation
- Employee benefits tracking

#### 12. **Communication System**
- Internal messaging system
- Email notifications
- SMS notifications (Twilio)
- Announcements system
- Parent-teacher communication
- Circular distribution
- Activity notifications

#### 13. **Reports & Analytics**
- Student performance reports
- Attendance reports & analytics
- Financial reports & statements
- Exam reports & grade analysis
- Payroll reports
- Hostel & transport reports
- Export to PDF/Excel/CSV
- Custom report builder

#### 14. **Settings & Configuration**
- School information management
- User account management
- Role & permission management
- System-wide settings
- Email template configuration
- Academic year management
- Backup & restore functionality

---

## **Technology Stack**

### **Backend**
- **Framework:** Laravel 12
- **Language:** PHP 8.3+
- **Database:** MySQL 8.0
- **Cache:** Redis 7.0+
- **Queue:** Redis Queue
- **Search:** MySQL Full-text Search

### **Frontend**
- **Templates:** Blade Templates
- **CSS Framework:** Tailwind CSS 3.0+
- **JavaScript:** Alpine.js 3.0+
- **Interactive Components:** Livewire 3.0+
- **Charts:** Chart.js / ApexCharts
- **Tables:** DataTables / Alpine Components

### **DevOps & Deployment**
- **Containerization:** Docker & Docker Compose
- **Web Server:** Nginx
- **Process Manager:** Supervisor
- **SSL:** Let's Encrypt with Certbot
- **Monitoring:** Netdata
- **Error Tracking:** Sentry

### **Additional Libraries**
- **Authentication:** Laravel Jetstream + Sanctum
- **Authorization:** Spatie Laravel Permission
- **PDF Export:** DomPDF
- **Excel Export:** Laravel Excel
- **Payment Gateways:** Stripe, Paystack, Flutterwave SDKs
- **SMS:** Twilio SDK
- **Error Tracking:** Sentry Laravel
- **Activity Logging:** Spatie Laravel Activitylog
- **Image Processing:** Intervention Image
- **Slugs:** Eloquent Sluggable
- **UUID:** Laravel UUID

---

## **System Requirements**

### **Server Requirements**
- PHP 8.3+ with FPM
- MySQL 8.0+
- Redis 6.0+
- Node.js 20+
- Composer 2.0+
- Nginx or Apache 2.4+

### **Supported Operating Systems**
- Ubuntu 22.04 LTS & 24.04 LTS
- Debian 11+
- CentOS 8+
- macOS (Intel & Apple Silicon)
- Windows (WSL2)

### **Hardware Requirements** (Minimum)
- **CPU:** 2 cores
- **RAM:** 4GB (8GB+ recommended)
- **Storage:** 20GB SSD
- **Bandwidth:** 1Mbps

### **Browser Support**
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## **Quick Start Guide**

### **Option 1: Local Development (5 minutes)**

#### Prerequisites
```bash
# Check PHP version
php -v
# Should be 8.3+

# Check Composer
composer --version

# Check Node.js
node -v && npm -v
```

#### Setup
```bash
# 1. Clone repository
git clone https://github.com/Mona-i/school-management-erp.git
cd school-management-erp

# 2. Install dependencies
composer install
npm install

# 3. Setup environment
cp .env.example .env
php artisan key:generate

# 4. Create database
mysql -u root -e "CREATE DATABASE school_erp;"

# 5. Update .env database credentials
nano .env
# Set: DB_USERNAME, DB_PASSWORD

# 6. Run migrations and seeders
php artisan migrate --seed

# 7. Build assets
npm run build

# 8. Start development servers (in separate terminals)
php artisan serve              # Terminal 1 - http://localhost:8000
npm run dev                    # Terminal 2
php artisan queue:work redis   # Terminal 3 (optional)
php artisan schedule:work      # Terminal 4 (optional)
```

**Access Application:** http://localhost:8000

### **Option 2: Docker Setup (3 minutes)**

#### Prerequisites
```bash
# Install Docker & Docker Compose from docker.com
docker --version
docker-compose --version
```

#### Setup
```bash
# 1. Clone repository
git clone https://github.com/Mona-i/school-management-erp.git
cd school-management-erp

# 2. Start containers
docker-compose up -d

# 3. Install dependencies and migrate
docker-compose exec app composer install
docker-compose exec app npm install
docker-compose exec app php artisan migrate --seed
docker-compose exec app npm run build

# 4. Verify all services
docker-compose ps
```

**Access Application:** http://localhost

**Services:**
- Application: http://localhost
- PhpMyAdmin: http://localhost:8081
- Mailhog: http://localhost:8025
- Redis Commander: http://localhost:8082

### **Option 3: Production Deployment**

See detailed guide in `docs/DEPLOYMENT.md`

---

## **Demo Credentials**

| Role | Email | Password | Access |
|------|-------|----------|--------|
| Super Admin | admin@school.test | password | http://localhost:8000 |
| School Admin | admin@school.test | password | http://localhost:8000 |
| Teacher | teacher@school.test | password | http://localhost:8000 |
| Student | student@school.test | password | http://localhost:8000 |
| Parent | parent@school.test | password | http://localhost:8000 |
| Accountant | accountant@school.test | password | http://localhost:8000 |
| Librarian | librarian@school.test | password | http://localhost:8000 |
| HR Manager | hr@school.test | password | http://localhost:8000 |

> **Note:** Change all passwords in production!

---

## **User Roles & Permissions**

### **8 User Roles**

1. **Super Admin** 
   - Full system access
   - Multi-school management
   - System settings
   - User management
   - Permission management

2. **School Admin**
   - School-level management
   - Staff management
   - Academic settings
   - Financial oversight
   - Reporting

3. **Teacher**
   - Class management
   - Student attendance
   - Grade entry
   - Assignment creation
   - Parent communication

4. **Student**
   - View grades & results
   - Submit assignments
   - Check attendance
   - View announcements
   - Download documents

5. **Parent**
   - Track student progress
   - View grades & attendance
   - Payment tracking
   - Parent-teacher communication
   - Download reports

6. **Accountant**
   - Finance management
   - Invoice generation
   - Payment processing
   - Expense tracking
   - Financial reports

7. **Librarian**
   - Library operations
   - Book inventory
   - Borrowing/returning
   - Fine calculation
   - Member management

8. **HR Manager**
   - Staff management
   - Leave processing
   - Performance tracking
   - Payroll management
   - Salary slip generation

---

## **Project Structure**

```
school-management-erp/
├── app/
│   ├── Models/              # 25+ Eloquent models
│   ├── Controllers/         # RESTful controllers
│   ├── Services/            # Business logic services
│   ├── Policies/            # Authorization policies
│   ├── Jobs/                # Queue jobs
│   ├── Events/              # Event classes
│   ├── Listeners/           # Event listeners
│   ├── Mail/                # Mailable classes
│   ├── Notifications/       # Notification classes
│   ├── Livewire/            # Livewire components
│   ├── Traits/              # Reusable traits
│   └── Exports/             # Excel exports
├── database/
│   ├── migrations/          # Database schemas (30+)
│   ├── seeders/             # Database seeders
│   └── factories/           # Model factories
├── resources/
│   ├── views/               # Blade templates (50+)
│   ├── js/                  # JavaScript files
│   └── css/                 # CSS files
├── routes/
│   ├── web.php              # Web routes
│   ├── api.php              # API routes
│   └── auth.php             # Auth routes
├── tests/
│   ├── Feature/             # Feature tests
│   ├── Unit/                # Unit tests
│   └── API/                 # API tests
├── docker/
│   ├── Dockerfile           # Docker image
│   ├── nginx.conf           # Nginx config
│   └── supervisord.conf     # Supervisor config
├── docs/
│   ├── DEPLOYMENT.md        # Deployment guide
│   ├── VIDEO_GUIDE.md       # Video tutorial
│   └── API.md               # API docs
├── storage/
│   ├── app/                 # File storage
│   ├── logs/                # Application logs
│   └── backups/             # Database backups
├── docker-compose.yml       # Docker services
├── .env.example             # Environment template
├── composer.json            # PHP dependencies
├── package.json             # Node dependencies
├── tailwind.config.js       # Tailwind config
└── README.md                # This file
```

---

## **Security Features**

✅ **CSRF Protection** - Token validation on all POST requests
✅ **XSS Prevention** - Input sanitization & output escaping
✅ **SQL Injection Prevention** - Parameterized queries via Eloquent
✅ **Two-Factor Authentication** - TOTP-based 2FA with recovery codes
✅ **Rate Limiting** - Throttle API endpoints & login attempts
✅ **Authorization Policies** - Gate & policy-based access control
✅ **Audit Logging** - Complete audit trail of all actions
✅ **Password Hashing** - bcrypt hashing with auto-upgrade
✅ **API Token Auth** - Sanctum tokens with expiration
✅ **Security Headers** - HSTS, CSP, X-Frame-Options
✅ **Data Encryption** - Encrypted at-rest sensitive fields
✅ **File Upload Security** - MIME type validation, virus scanning ready

---

## **API Documentation**

### **Authentication**
```bash
# Get token
POST /api/login
{
  "email": "user@school.test",
  "password": "password"
}

# Use in requests
Authorization: Bearer {token}
```

### **Student Endpoints**
```
GET    /api/students               # List all students
POST   /api/students               # Create student
GET    /api/students/{id}          # Get student
PUT    /api/students/{id}          # Update student
DELETE /api/students/{id}          # Delete student
GET    /api/students/{id}/grades   # Get student grades
GET    /api/students/{id}/attendance # Get attendance
```

### **Attendance Endpoints**
```
GET    /api/attendance             # List attendance
POST   /api/attendance/mark        # Mark attendance
GET    /api/attendance/report      # Get report
POST   /api/attendance/qr-scan     # QR code scan
```

### **Payment Endpoints**
```
GET    /api/payments               # List payments
POST   /api/payments               # Create payment
GET    /api/payments/{id}          # Get payment
POST   /api/payments/stripe        # Stripe payment
POST   /api/payments/paystack      # Paystack payment
GET    /api/payments/export        # Export payments
```

**Full API Documentation:** See `docs/API.md`

---

## **Installation & Configuration**

### **Environment Setup**

Edit `.env` file:
```env
APP_NAME="School Management ERP"
APP_ENV=production
APP_DEBUG=false
APP_URL=https://your-domain.com

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=school_erp
DB_USERNAME=school_user
DB_PASSWORD=your_strong_password

# Cache & Queue
CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# Mail
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=465
MAIL_USERNAME=your_username
MAIL_PASSWORD=your_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@school-erp.com

# Payment Gateways
STRIPE_PUBLIC_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
PAYSTACK_PUBLIC_KEY=pk_test_...
PAYSTACK_SECRET_KEY=sk_test_...

# SMS (Twilio)
TWILIO_SID=your_sid
TWILIO_AUTH_TOKEN=your_token
TWILIO_PHONE_NUMBER=+1234567890

# Error Tracking (Sentry)
SENTRY_LARAVEL_DSN=your_sentry_dsn
```

### **Database Setup**
```bash
# Create database
mysql -u root -e "CREATE DATABASE school_erp;"

# Run migrations
php artisan migrate

# Seed data
php artisan db:seed

# Verify
mysql school_erp -u root -e "SHOW TABLES;"
```

---

## **Testing**

```bash
# Run all tests
php artisan test

# Run specific test file
php artisan test tests/Feature/StudentAdmissionTest.php

# Run with coverage
php artisan test --coverage

# Run only failing tests
php artisan test --repeat=3
```

---

## **Deployment**

### **VPS Deployment (Ubuntu 22.04)**
Follow detailed step-by-step guide in `docs/DEPLOYMENT.md`
- Server setup (15 min)
- Install dependencies (20 min)
- Deploy application (25 min)
- Configure Nginx (12 min)
- Setup SSL (10 min)
- Configure Supervisor (12 min)
- Setup backups (10 min)
- Total: ~2 hours

### **Docker Deployment**
```bash
# Development
docker-compose up -d

# Production (Docker Swarm)
docker swarm init
docker stack deploy -c docker-compose.yml school-erp
```

### **AWS Deployment**
See Elastic Beanstalk guide in `docs/DEPLOYMENT.md`

---

## **Video Tutorial Series**

Comprehensive **23-part video guide** (6-7 hours total):

**Part 1-5:** Foundation & Setup (1h 40m)
- Overview & features demo
- Local development setup
- Docker setup
- Database & seeding
- Authentication & 2FA

**Part 6-12:** Core Modules (2h 20m)
- Student management
- Teacher management
- Academic management
- Attendance system
- Exam & grading
- Finance & payments
- Library & hostel

**Part 13-17:** Advanced Features (1h 35m)
- Reports & analytics
- Dashboard & charts
- Settings & configuration
- API & mobile integration
- Advanced integrations

**Part 18-23:** Production Deployment (2h 50m)
- VPS deployment (45 min)
- Docker production (30 min)
- Monitoring & maintenance (25 min)
- Advanced topics (30 min)
- Troubleshooting (25 min)
- Best practices (18 min)

See `docs/VIDEO_GUIDE.md` for complete details and timestamps.

---

## **Documentation**

- **[DEPLOYMENT.md](docs/DEPLOYMENT.md)** - Complete production deployment guide (8000+ words)
- **[VIDEO_GUIDE.md](docs/VIDEO_GUIDE.md)** - 23-part video tutorial series (12000+ words)
- **[API.md](docs/API.md)** - Comprehensive API documentation
- **[ARCHITECTURE.md](docs/ARCHITECTURE.md)** - System architecture & design patterns
- **[SETUP.md](docs/SETUP.md)** - Local development setup guide
- **[TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)** - Common issues & solutions

---

## **Support & Community**

- **GitHub Issues:** Report bugs and request features
- **Email:** support@school-erp.com
- **Documentation:** https://github.com/Mona-i/school-management-erp/wiki
- **Discussions:** GitHub Discussions for community help

---

## **Contributing**

We welcome contributions! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## **License**

This project is licensed under the MIT License - see [LICENSE.md](LICENSE.md) file for details.

---

## **Changelog**

See [CHANGELOG.md](CHANGELOG.md) for version history and updates.

---

## **Roadmap**

- [x] Core authentication & authorization
- [x] Student management system
- [x] Academic management
- [x] Attendance tracking
- [x] Exam & grading system
- [x] Finance & payments
- [ ] Mobile app (React Native)
- [ ] Advanced analytics & BI dashboards
- [ ] Multi-language support (i18n)
- [ ] Multi-tenancy support
- [ ] AI-powered recommendations
- [ ] Video conferencing integration

---

## **Credits & Acknowledgments**

Built with ❤️ for the education industry.

**Technologies & Frameworks:**
- Laravel Community
- PHP Foundation
- Tailwind Labs
- Alpine.js Team
- Livewire Team

**Special Thanks:**
- All contributors and community members
- Open source libraries & frameworks
- Education professionals for feedback

---

## **Getting Help**

1. **Check Documentation** - Most answers in docs/
2. **Search Issues** - Your question might be answered
3. **Read FAQ** - Common questions answered
4. **Ask in Discussions** - Community help available
5. **Email Support** - For professional support

---

**Start your School Management ERP journey today! 🚀**

Made with ❤️ by the Mona-i Development Team
