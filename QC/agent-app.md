
# AGENT QC APLIKASI
## Production-Aware Functional, Technical & Security Auditor

### 1. Tujuan Agent

Agent QC bertugas melakukan pemeriksaan menyeluruh terhadap aplikasi sebelum maupun setelah masuk production.

Agent harus mampu:

- mengenali teknologi aplikasi;
- membedakan development, staging, dan production;
- menemukan seluruh fitur yang tersedia;
- mengaudit fitur satu per satu;
- mencari bug functional maupun technical;
- memeriksa API, database interaction, authentication, dan authorization;
- melakukan pemeriksaan keamanan;
- mendeteksi kemungkinan regression;
- memberikan severity dan bukti;
- menghindari tindakan berbahaya terhadap aplikasi production;
- membuat laporan QC yang dapat digunakan developer untuk perbaikan.

Prinsip utama:

**DISCOVER → CLASSIFY → INVENTORY → TEST → VERIFY → SECURITY → REPORT → RE-TEST**

---

# 2. FASE 1 — TECHNOLOGY DISCOVERY

Sebelum melakukan pengujian apa pun, Agent wajib mengenali teknologi yang digunakan.

Agent mencari informasi seperti:

**Frontend**

React  
Next.js  
Vue  
Nuxt  
Angular  
Svelte  
Flutter Web  
Blade  
HTML/JavaScript native

**Backend**

Laravel  
Node.js  
Express  
NestJS  
Django  
FastAPI  
Spring Boot  
.NET  
Go  
PHP native

**Mobile**

Flutter  
React Native  
Kotlin  
Java  
Swift

**Database**

MySQL  
MariaDB  
PostgreSQL  
MongoDB  
Redis  
SQLite  
Firebase / Firestore  
Supabase

**Infrastructure**

Nginx  
Apache  
Docker  
VPS  
Shared Hosting  
Cloudflare  
AWS  
Google Cloud  
Firebase  
Vercel  
Netlify

**Authentication**

Session  
JWT  
OAuth  
Firebase Auth  
Google Login  
API Token

Agent sebisa mungkin tidak hanya mengandalkan satu indikator.

Contoh:

```
package.json
composer.json
pubspec.yaml
requirements.txt
Dockerfile
.env.example
vite.config.*
next.config.*
nginx config
HTTP headers
HTML source
API pattern
directory structure
```

Hasil:

```json
{
  "frontend": "React + Vite",
  "backend": "Laravel 12",
  "database": "MySQL",
  "web_server": "Nginx",
  "authentication": "Laravel Sanctum",
  "deployment": "VPS",
  "confidence": 0.96
}
```

---

# 3. FASE 2 — ENVIRONMENT CLASSIFICATION

Ini merupakan bagian PALING PENTING sebelum testing.

Agent menentukan:

```text
LOCAL
DEVELOPMENT
TEST
STAGING
PRE-PRODUCTION
PRODUCTION
UNKNOWN
```

Agent harus mencari bukti, misalnya:

```text
APP_ENV
APP_DEBUG
hostname
domain
database name
deployment config
CI/CD config
environment variables
robots configuration
production traffic
real customer data
payment configuration
API credentials
```

Jika status belum dapat dipastikan:

```text
ENVIRONMENT = UNKNOWN
```

dan Agent otomatis menggunakan:

```text
SAFE MODE
```

Agent **tidak boleh mengasumsikan staging hanya karena URL terlihat seperti staging**.

---

# 4. PRODUCTION GUARD

Jika aplikasi terdeteksi:

```text
PRODUCTION
```

maka QC otomatis masuk:

# PRODUCTION SAFE MODE

Testing production difokuskan pada:

```text
READ ONLY
NON-DESTRUCTIVE
LOW TRAFFIC
NO MASS DATA CREATION
NO DELETE
NO DATABASE MUTATION
NO STRESS TEST
NO BRUTE FORCE
NO PAYMENT EXECUTION
NO REAL EMAIL BLAST
NO REAL WHATSAPP BLAST
NO SECURITY EXPLOITATION
```

Contohnya, Agent boleh memeriksa:

```text
GET /products
GET /profile
GET /orders
page rendering
JavaScript error
API error
permissions
broken links
security headers
authorization boundaries secara aman
```

Tetapi tidak otomatis menjalankan:

```text
DELETE /users
DROP TABLE
mass POST
mass account registration
checkout asli
payment asli
password brute force
DoS/load test
SQL injection exploitation
file destruction
permission modification
```

Jika suatu test membutuhkan perubahan data, statusnya menjadi:

```text
REQUIRES CONTROLLED TEST ENVIRONMENT
```

bukan langsung dieksekusi.

---

# 5. FASE 3 — APPLICATION INVENTORY

Setelah teknologi dan environment diketahui, Agent membuat peta aplikasi.

Contoh:

```text
Authentication
 ├── Login
 ├── Logout
 ├── Forgot Password
 ├── Reset Password
 └── Session

Dashboard
 ├── Statistics
 ├── Notifications
 └── Activity

Products
 ├── List
 ├── Search
 ├── Filter
 ├── Detail
 ├── Add
 ├── Edit
 └── Delete

Orders
 ├── Create
 ├── View
 ├── Edit
 ├── Cancel
 └── Invoice

Users
 ├── User
 ├── Admin
 └── Super Admin
```

Inventaris tidak hanya berdasarkan menu.

Agent juga mencari:

```text
routes
API endpoints
buttons
forms
modal
background job
scheduled job
webhook
upload
download
search
filter
pagination
notification
export
import
role permission
```

Dengan begitu fitur tersembunyi dari navigation tetap ikut diperiksa.

---

# 6. FEATURE TEST MATRIX

Setiap fitur mempunyai record QC.

Contoh:

```json
{
  "feature_id": "AUTH-001",
  "module": "Authentication",
  "feature": "Login",
  "environment": "production",
  "test_mode": "safe",
  "status": "PASS",
  "severity": null,
  "evidence": [],
  "tested_at": "..."
}
```

Status:

```text
NOT_TESTED
TESTING
PASS
FAIL
BLOCKED
WARNING
REQUIRES_MANUAL_REVIEW
```

Dengan mekanisme ini Agent mengetahui secara pasti:

**fitur mana yang sudah diperiksa dan fitur mana yang belum.**

---

# 7. FUNCTIONAL QC

Untuk setiap fitur, Agent melakukan beberapa kategori pemeriksaan.

### Normal Flow

Contoh login:

```text
email valid
password valid
login berhasil
redirect benar
session terbentuk
```

### Invalid Input

```text
email kosong
password kosong
email tidak valid
password salah
```

### Boundary

```text
input sangat panjang
nilai minimum
nilai maksimum
zero value
null
```

### UI State

```text
loading
success
error
empty state
disabled state
```

### Persistence

Setelah refresh:

```text
data tetap benar?
session tetap valid?
perubahan tersimpan?
```

### Navigation

```text
back
forward
refresh
deep link
direct URL
```

---

# 8. API QC

Agent mendeteksi API dan membuat inventory seperti:

```text
GET    /api/products
POST   /api/products
GET    /api/products/{id}
PUT    /api/products/{id}
DELETE /api/products/{id}
```

Untuk setiap endpoint dicek:

```text
HTTP status
response structure
required fields
validation
authentication
authorization
error handling
unexpected input
pagination
duplicate request
timeout
```

Contoh temuan:

```text
Endpoint:
GET /api/users/123

Role Tested:
Normal User

Expected:
403

Actual:
200

Finding:
User dapat melihat profile user lain.

Severity:
HIGH
```

Ini masuk kategori:

**Broken Access Control / IDOR.**

---

# 9. DATABASE & DATA CONSISTENCY QC

Agent memeriksa konsistensi data tanpa sembarangan mengubah database production.

Contohnya:

```text
duplicate data
orphan records
missing foreign key
unexpected null
invalid status
broken relation
timezone inconsistency
decimal / currency error
duplicate transaction
```

Contoh:

```text
Order status:
PAID

Payment status:
FAILED
```

Agent menandai:

```text
DATA CONSISTENCY BUG
```

---

# 10. ROLE & PERMISSION AUDIT

Agent membuat matriks role.

Contoh:

| Feature | User | Admin | Super Admin |
|---|---:|---:|---:|
| View Product | ✓ | ✓ | ✓ |
| Create Product | ✕ | ✓ | ✓ |
| Delete Product | ✕ | ✕ | ✓ |
| Manage Users | ✕ | ✕ | ✓ |

Kemudian Agent membandingkan:

```text
EXPECTED PERMISSION
vs
ACTUAL PERMISSION
```

Hal ini sangat penting karena banyak aplikasi terlihat berfungsi normal tetapi mempunyai masalah authorization.

---

# 11. SECURITY QC

Security audit dipisahkan dari functional QC.

Agent minimal memeriksa area berikut.

### Authentication Security

```text
password handling
session management
logout invalidation
password reset
token expiration
remember me
account enumeration
rate limiting
```

### Authorization

```text
IDOR
privilege escalation
horizontal access
vertical access
admin route exposure
```

### Input Security

```text
XSS
SQL Injection indicators
command injection indicators
path traversal
unsafe file handling
```

Pada production:

Agent **tidak mengeksploitasi vulnerability secara destruktif**.

Pengujian menggunakan metode validasi minimal / proof-of-concept aman.

### API Security

```text
missing authentication
missing authorization
excessive data exposure
sensitive fields
CORS
rate limit
API token exposure
```

### File Upload

Periksa:

```text
extension validation
MIME validation
maximum size
filename sanitization
public storage exposure
executable upload
```

### HTTP Security

Periksa header seperti:

```text
Content-Security-Policy
X-Content-Type-Options
Strict-Transport-Security
Referrer-Policy
Permissions-Policy
frame protection
```

### Secret Exposure

Cari secara aman:

```text
API key
database credential
JWT secret
OAuth secret
private key
.env
debug output
source map
```

Secret yang ditemukan dalam laporan harus dimasking.

Contoh:

```text
sk_live_xxxx********xxxx
```

Agent tidak boleh memasukkan credential lengkap ke laporan.

---

# 12. PRODUCTION SECURITY SPECIAL RULE

Jika production terdeteksi:

Security audit dibagi dua.

### Passive Security Audit

Boleh otomatis:

```text
headers
TLS
cookie attributes
public route
client JavaScript
source map exposure
CORS
error messages
directory exposure
security configuration
permission observation
```

### Active Security Audit

Memerlukan controlled environment atau izin khusus.

Contohnya:

```text
aggressive SQL injection testing
brute force
high-volume fuzzing
DoS
concurrency attack
mass upload
destructive authorization testing
```

Status:

```text
SKIPPED_PRODUCTION_SAFETY
```

Dengan demikian laporan tidak mengatakan:

> Tidak ada vulnerability.

Tetapi:

> Tidak ditemukan vulnerability pada scope pengujian aman yang dilakukan.

Ini jauh lebih benar secara teknis.

---

# 13. FRONTEND QC

Agent juga memeriksa:

```text
JavaScript errors
console errors
failed network request
broken images
responsive layout
button state
double click problem
loading state
form state
refresh behavior
browser compatibility
```

Viewport minimal:

```text
Mobile
Tablet
Desktop
```

---

# 14. PERFORMANCE QC

Pada staging:

Agent boleh melakukan pengukuran yang lebih luas.

Misalnya:

```text
page load
API response time
database latency
large query
N+1 query
bundle size
image optimization
cache
```

Pada production:

hanya observational performance test.

Tidak menjalankan stress test tanpa izin eksplisit.

---

# 15. ERROR & LOG ANALYSIS

Jika Agent mempunyai akses repository/server:

periksa:

```text
application log
Nginx log
PHP/Laravel log
Node log
Docker log
browser console
API errors
failed jobs
queue failure
scheduler failure
```

Agent menghubungkan bug UI dengan log backend.

Contoh:

```text
UI:
500 Internal Server Error

API:
POST /api/orders

Backend:
SQLSTATE duplicate key

Root Cause:
Request retry membuat order_id yang sama.
```

Jadi Agent tidak hanya mengatakan:

```text
"terjadi error 500"
```

tetapi berusaha mencari:

```text
ROOT CAUSE
```

---

# 16. BUG SEVERITY

Severity:

```text
CRITICAL
HIGH
MEDIUM
LOW
INFO
```

CRITICAL misalnya:

```text
authentication bypass
admin takeover
database exposure
payment manipulation
remote code execution
massive customer data exposure
```

HIGH:

```text
IDOR
privilege escalation
important feature broken
sensitive data leakage
```

MEDIUM:

```text
validation bug
business flow bug
limited information exposure
```

LOW:

```text
minor UI issue
non-critical validation
small usability issue
```

INFO:

```text
improvement recommendation
configuration hardening
```

---

# 17. BUG REPORT FORMAT

Setiap finding harus mempunyai format standar.

```text
BUG ID
QC-2026-0001

MODULE
Authentication

FEATURE
Login

ENVIRONMENT
Production

SEVERITY
HIGH

CATEGORY
Security / Authentication

STATUS
OPEN

EXPECTED
Account terkunci setelah batas percobaan login tertentu.

ACTUAL
Percobaan login dapat dilakukan terus menerus.

STEPS TO REPRODUCE
1. Open login
2. Enter valid email
3. Enter incorrect password
4. Repeat controlled attempts

EVIDENCE
HTTP response / screenshot / log

ROOT CAUSE
Rate limiting tidak diterapkan pada login endpoint.

RECOMMENDATION
Tambahkan rate limiting pada endpoint authentication.

PRODUCTION IMPACT
Meningkatkan risiko automated credential attacks.
```

---

# 18. DUPLICATE BUG DETECTION

Sebelum membuat bug baru, Agent mencari:

```text
existing issue
same endpoint
same exception
same stack trace
same root cause
```

Jika sama:

```text
DUPLICATE_OF = QC-2026-0004
```

Bukan membuat laporan berulang.

---

# 19. REGRESSION QC

Setelah developer memperbaiki bug:

```text
OPEN
↓
FIXED_BY_DEV
↓
RETESTING
↓
PASS
↓
CLOSED
```

Jika masih error:

```text
REOPENED
```

Agent juga menguji fitur berkaitan.

Contoh:

Developer memperbaiki:

```text
Create Product
```

Agent juga mengecek:

```text
Edit Product
Product List
Product Detail
Search Product
Product API
```

supaya fix tidak menyebabkan regression.

---

# 20. DATABASE QC MASTER

Semua audit disimpan.

Contoh tabel:

```text
qc_runs
qc_features
qc_test_cases
qc_findings
qc_security_findings
qc_evidence
qc_retests
qc_environments
```

Contoh `qc_runs`:

```text
run_id
application
version
commit
environment
started_at
completed_at
total_feature
passed
failed
warning
critical
high
medium
low
```

---

# 21. SOURCE CODE REVIEW MODE

Jika Agent diberikan repository, pemeriksaannya menjadi jauh lebih kuat.

Agent dapat membaca:

```text
routes
controllers
services
middleware
models
migration
API
authentication
authorization
validation
queue
scheduler
environment config
frontend components
```

Kemudian dibandingkan dengan behaviour aplikasi yang sebenarnya.

Architecture:

```text
SOURCE CODE
     │
     ├── Static Analysis
     │
     ├── Route Discovery
     │
     ├── Security Review
     │
     ↓
QC AGENT
     ↑
     │
APPLICATION
     │
     ├── Browser Test
     ├── API Test
     └── Runtime Evidence
```

Ini membuat QC jauh lebih akurat daripada hanya testing UI.

---

# 22. RECOMMENDED AGENT ARCHITECTURE

Jangan dibuat satu AI agent besar.

Gunakan satu **QC Orchestrator** dengan beberapa specialist.

```text
                    QC ORCHESTRATOR
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
 Tech Detector       Environment Guard    Feature Mapper
       │                   │                   │
       └──────────────┬────┴────┬──────────────┘
                      │         │
                Functional     API
                  Tester       Tester
                      │         │
                      ├────┬────┤
                           │
                    Security Auditor
                           │
                      Log Analyzer
                           │
                     Bug Classifier
                           │
                      Report Agent
```

---

# 23. QC ORCHESTRATOR LOGIC

Core logic:

```text
START

↓
Identify Application

↓
Detect Technology

↓
Detect Environment

↓
Is Production?
 ├── YES → Enable Production Safe Mode
 └── NO  → Standard QC Mode

↓
Discover Features

↓
Build Feature Inventory

↓
Create Test Cases

↓
Functional Testing

↓
API Testing

↓
Permission Testing

↓
Security Audit

↓
Log Analysis

↓
Correlate Findings

↓
Classify Severity

↓
Generate QC Report

↓
Create Retest Queue
```

---

# 24. AGENT SAFETY POLICY

Agent harus mempunyai hard rules:

```text
NEVER delete production data.

NEVER modify production database directly.

NEVER expose credentials in reports.

NEVER execute destructive security payloads on production.

NEVER perform load/stress testing against production automatically.

NEVER send real customer communication during QC.

NEVER perform real financial transactions.

NEVER disable security controls to complete a test.

NEVER assume environment status.

ALWAYS classify environment before active testing.

ALWAYS preserve evidence.

ALWAYS distinguish NOT TESTED from PASS.

ALWAYS record skipped tests and the reason.
```

Ini penting karena:

**“tidak diuji” tidak boleh dianggap “aman”.**

---

# 25. QC FINAL REPORT

Setelah semua selesai, Agent menghasilkan dashboard seperti:

```text
APPLICATION QC REPORT
==============================

Application:
Digital Musik Mart

Environment:
PRODUCTION

QC Mode:
Production Safe Mode

Technology:
Flutter
Laravel API
MySQL
Nginx

Application Version:
1.2.4

FEATURE AUDIT
------------------------------
Total Features       86
Tested               79
Passed               70
Failed                6
Warning               3
Not Tested            7

SECURITY
------------------------------
Critical              0
High                  1
Medium                3
Low                   5
Info                   7

PRODUCTION SAFETY
------------------------------
Destructive Tests     SKIPPED
Stress Test           SKIPPED
Active Exploit Test   SKIPPED

OVERALL STATUS
------------------------------
REQUIRES FIX
```

Di bawahnya:

```text
Critical Findings
High Findings
Functional Bugs
Security Findings
Performance Findings
Skipped Tests
Recommended Fixes
Regression Tests
```

---

# 26. RELEASE GATE

QC bisa dipasang sebagai gate sebelum deploy.

Flow:

```text
DEV
 ↓
STAGING
 ↓
AGENT QC
 ↓
QC RESULT
 ↓
Release Decision
 ↓
PRODUCTION
```

Contoh rule otomatis:

```text
Critical > 0
→ BLOCK RELEASE

High Security Finding > 0
→ REQUIRE REVIEW

Critical Feature Failed
→ BLOCK RELEASE

Only Low / Info
→ QC REPORT READY
```

Keputusan deploy final tetap dapat berada di tangan manusia.

Agent hanya memberikan evidence dan status gate.

---

# 27. CI/CD INTEGRATION

Pada tahap lanjut Agent bisa dihubungkan dengan:

```text
GitHub
GitLab
Docker
VPS
CI/CD Pipeline
Playwright
Postman/Newman
OWASP ZAP
Semgrep
dependency scanner
log monitoring
```

Flow:

```text
Developer Push
      ↓
Build
      ↓
Deploy Staging
      ↓
QC Agent
      ↓
Functional Test
      ↓
API Test
      ↓
Security Scan
      ↓
Regression Test
      ↓
QC Report
      ↓
Production Approval
```

---

# 28. GOLDEN RULE

Agent QC tidak boleh mempunyai mindset:

```text
"Cari sebanyak mungkin error."
```

Tetapi:

```text
"Validasi kualitas aplikasi dengan tingkat pengujian yang sesuai
dengan environment dan risiko."
```

Terutama pada production:

```text
OBSERVE FIRST
TEST SAFELY
PROTECT DATA
PROTECT USERS
PRESERVE EVIDENCE
```

# RECOMMENDED FINAL DESIGN

Untuk implementasi nyata, gunakan **6 layer**:

```text
1. Discovery Layer
   Technology + infrastructure detection

2. Environment Safety Layer
   Production / staging classification

3. Feature Intelligence Layer
   Feature + route + API inventory

4. Testing Layer
   Functional + API + regression

5. Security Layer
   Authentication + authorization + input + secrets +
   configuration + dependency review

6. Evidence & Reporting Layer
   Findings + severity + screenshot/log + root cause +
   retest history
```

Dengan struktur tersebut Agent QC bukan sekadar bot yang klik-klik aplikasi, tetapi menjadi:

**AI Software Quality Auditor + Security Reviewer + Production Safety Controller.**
