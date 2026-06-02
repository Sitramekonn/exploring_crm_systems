# Part 4 – CRM Architecture Analysis

**Course:** ICS499 – Software Engineering and Capstone Project  
**Stack:** HTML + CSS + Bootstrap + JavaScript + jQuery → PHP → MySQL  
**AI Tool Used:** Claude (Anthropic)

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   BROWSER LAYER                     │
│   HTML/CSS   Bootstrap   JavaScript   jQuery        │
│              Chart.js    DataTables                 │
└───────────────────────┬─────────────────────────────┘
                        │  HTTP Requests / Responses
                        ▼
┌─────────────────────────────────────────────────────┐
│              PHP APPLICATION LAYER                  │
│  Auth  │ Contacts │ Leads │ Activities │ Reports    │
│        PHPMailer │ PDO │ Sessions │ bcrypt          │
└───────────────────────┬─────────────────────────────┘
                        │  PDO Queries / Result Sets
                        ▼
┌─────────────────────────────────────────────────────┐
│                 MYSQL DATABASE                      │
│  users │ contacts │ leads │ opportunities           │
│  activities │ accounts │ tasks │ audit_log          │
└─────────────────────────────────────────────────────┘
```

See `architecture/crm_architecture.png` for the visual diagram.

---

## Functional Modules

A CRM built on this stack should include the following modules:

| Module | Description | Key PHP File(s) |
|--------|-------------|----------------|
| **Authentication** | Login, logout, password reset, session management | `auth/login.php`, `auth/logout.php` |
| **Dashboard** | Overview of KPIs, recent activities, pipeline summary | `dashboard.php` |
| **Contacts** | Create, read, update, delete customer contact records | `contacts/index.php`, `contacts/edit.php` |
| **Accounts** | Company/organization records linked to contacts | `accounts/index.php` |
| **Leads** | Unqualified prospects; track through qualification pipeline | `leads/index.php`, `leads/convert.php` |
| **Opportunities** | Qualified deals; track through stages to closed/won | `opportunities/index.php` |
| **Activities** | Log calls, emails, meetings linked to contacts/leads | `activities/index.php` |
| **Tasks** | Assignable to-do items with due dates and priority | `tasks/index.php` |
| **Reports** | Generate sales pipeline, lead conversion, activity reports | `reports/index.php` |
| **User Management** | Admin panel to manage users and roles | `admin/users.php` |

---

## Database Design

### Core Tables

#### `users`
```sql
CREATE TABLE users (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(100) NOT NULL,
    email       VARCHAR(150) NOT NULL UNIQUE,
    password    VARCHAR(255) NOT NULL,   -- bcrypt hash
    role        ENUM('admin','manager','user') DEFAULT 'user',
    is_active   TINYINT(1) DEFAULT 1,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

#### `contacts`
```sql
CREATE TABLE contacts (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    first_name  VARCHAR(100) NOT NULL,
    last_name   VARCHAR(100) NOT NULL,
    email       VARCHAR(150),
    phone       VARCHAR(20),
    account_id  INT,                     -- FK → accounts.id
    assigned_to INT,                     -- FK → users.id
    notes       TEXT,
    created_by  INT,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (account_id) REFERENCES accounts(id) ON DELETE SET NULL,
    FOREIGN KEY (assigned_to) REFERENCES users(id) ON DELETE SET NULL
);
```

#### `accounts`
```sql
CREATE TABLE accounts (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(200) NOT NULL,
    industry    VARCHAR(100),
    website     VARCHAR(200),
    phone       VARCHAR(20),
    address     TEXT,
    assigned_to INT,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (assigned_to) REFERENCES users(id) ON DELETE SET NULL
);
```

#### `leads`
```sql
CREATE TABLE leads (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    first_name  VARCHAR(100) NOT NULL,
    last_name   VARCHAR(100) NOT NULL,
    email       VARCHAR(150),
    phone       VARCHAR(20),
    company     VARCHAR(200),
    source      ENUM('web','referral','cold_call','email','other'),
    status      ENUM('new','assigned','in_process','converted','recycled','dead') DEFAULT 'new',
    assigned_to INT,
    converted   TINYINT(1) DEFAULT 0,
    converted_at TIMESTAMP NULL,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (assigned_to) REFERENCES users(id) ON DELETE SET NULL
);
```

#### `opportunities`
```sql
CREATE TABLE opportunities (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(200) NOT NULL,
    account_id  INT,
    contact_id  INT,
    stage       ENUM('prospecting','qualification','proposal','negotiation','closed_won','closed_lost') DEFAULT 'prospecting',
    amount      DECIMAL(12,2),
    close_date  DATE,
    probability INT DEFAULT 0,
    assigned_to INT,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (account_id) REFERENCES accounts(id) ON DELETE SET NULL,
    FOREIGN KEY (assigned_to) REFERENCES users(id) ON DELETE SET NULL
);
```

#### `activities`
```sql
CREATE TABLE activities (
    id           INT AUTO_INCREMENT PRIMARY KEY,
    type         ENUM('call','email','meeting','note','task'),
    subject      VARCHAR(200),
    description  TEXT,
    contact_id   INT,
    lead_id      INT,
    opportunity_id INT,
    due_date     DATETIME,
    status       ENUM('planned','completed','cancelled') DEFAULT 'planned',
    assigned_to  INT,
    created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (contact_id) REFERENCES contacts(id) ON DELETE SET NULL,
    FOREIGN KEY (assigned_to) REFERENCES users(id) ON DELETE SET NULL
);
```

#### `roles` (optional for fine-grained permissions)
```sql
CREATE TABLE roles (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(50) NOT NULL UNIQUE,
    permissions JSON
);
```

---

## Useful Libraries

| Library | Purpose | How Used |
|---------|---------|---------|
| **Bootstrap 5** | Responsive CSS framework | Navigation, forms, modals, cards, grid layout — entire UI structure |
| **jQuery** | JavaScript DOM manipulation | AJAX calls to PHP endpoints, dynamic form validation, UI interactions |
| **DataTables** | Interactive HTML tables | Sortable, searchable, paginated tables for Contacts, Leads, Opportunities lists |
| **Chart.js** | JavaScript charting | Dashboard pie/bar/line charts for pipeline by stage, leads by source, monthly revenue |
| **PHPMailer** | Email sending from PHP | Send automated follow-up reminders, lead assignment notifications, password reset emails |
| **Composer** | PHP dependency manager | Manages PHPMailer and other PHP packages; autoloads classes |

### Installation Examples

**Composer (PHPMailer):**
```bash
composer require phpmailer/phpmailer
```

**Bootstrap + jQuery + Chart.js (CDN):**
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
```

---

## Security Considerations

### Authentication

- Sessions created with `session_start()` after successful login
- Session IDs regenerated with `session_regenerate_id(true)` after login to prevent session fixation
- Sessions expire after a configurable timeout (e.g., 30 minutes of inactivity)

```php
// Example: secure login
session_start();
session_regenerate_id(true);
$_SESSION['user_id'] = $user['id'];
$_SESSION['role'] = $user['role'];
$_SESSION['last_activity'] = time();
```

### Authorization

- Role-based access control (RBAC) checked at the top of every PHP page
- Admin role: full access to all records and user management
- Manager role: view/edit all records, no user management
- User role: view/edit only their own assigned records

```php
// Example: authorization check
if ($_SESSION['role'] !== 'admin') {
    header('Location: /dashboard.php');
    exit;
}
```

### Password Security

- Passwords hashed with `password_hash($password, PASSWORD_BCRYPT)` — never stored in plain text
- Verified with `password_verify($input, $hash)` on login
- Minimum password length enforced (8+ characters)
- Password reset uses time-limited tokens stored in a `password_resets` table

### SQL Injection Prevention

- All database queries use **PDO with prepared statements** — no raw string concatenation
- Never use `mysqli_query()` with unescaped user input

```php
// Safe: prepared statement
$stmt = $pdo->prepare("SELECT * FROM contacts WHERE email = ?");
$stmt->execute([$email]);
$contact = $stmt->fetch();

// UNSAFE (never do this):
// $result = mysqli_query($conn, "SELECT * FROM contacts WHERE email = '$email'");
```

### Cross-Site Scripting (XSS) Prevention

- All output rendered to HTML escaped with `htmlspecialchars()` before display
- Content Security Policy (CSP) header set to restrict script sources

```php
// Safe output
echo htmlspecialchars($contact['name'], ENT_QUOTES, 'UTF-8');
```

### Data Privacy

- HTTPS enforced in production (SSL certificate required)
- Sensitive fields (passwords, API keys) never logged or exposed in error messages
- Database credentials stored in a `.env` file or `config.php` outside the web root
- `config.php` excluded from version control via `.gitignore`

---

## MVP Proposal — Version 1

The smallest useful CRM that could be built in one sprint (2–4 weeks):

### MVP Feature Set

| Feature | Included in V1 | Rationale |
|---------|---------------|-----------|
| User login / logout | ✅ | Security baseline — required |
| Contact management (CRUD) | ✅ | Core CRM function |
| Lead list with status | ✅ | Primary sales workflow |
| Activity logging | ✅ | Track interactions |
| Basic dashboard | ✅ | Show KPIs at a glance |
| Account management | ⬜ V2 | Add after contacts proven |
| Opportunity pipeline | ⬜ V2 | Needs accounts first |
| Email notifications | ⬜ V2 | PHPMailer setup not critical for V1 |
| Reports module | ⬜ V2 | Add once data exists |
| User roles / admin panel | ⬜ V2 | Start with single admin user |

### V1 File Structure

```
crm/
├── config/
│   └── database.php         # PDO connection
├── auth/
│   ├── login.php
│   └── logout.php
├── contacts/
│   ├── index.php            # List all contacts
│   ├── create.php           # New contact form
│   ├── edit.php             # Edit form
│   └── delete.php           # Delete handler
├── leads/
│   ├── index.php
│   ├── create.php
│   └── edit.php
├── activities/
│   ├── index.php
│   └── create.php
├── dashboard.php            # KPI overview
├── includes/
│   ├── header.php           # Bootstrap nav
│   └── footer.php
└── assets/
    ├── css/custom.css
    └── js/main.js
```

### V1 Dashboard KPIs

- Total contacts
- New leads this week
- Activities due today
- Open leads by status (Chart.js doughnut)

---

## AI Research Reflection

> **Tool used:** Claude (Anthropic)  
> **Prompts used:** See `prompts_used.md` for the architecture-specific prompts  
> **Response quality:** Excellent — Claude generated complete SQL table definitions, security code examples, and library recommendations that were accurate and directly usable.  
> **Validation:** SQL syntax verified by running CREATE TABLE statements in a local MySQL environment. PHP security examples cross-referenced against PHP official documentation (php.net) and OWASP guidelines.  
> **Trustworthiness:** High for architecture patterns and security fundamentals — these are stable, well-documented practices. Library version numbers were verified against official CDN pages.
