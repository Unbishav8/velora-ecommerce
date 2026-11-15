# Velora E-Commerce

Velora is a modern, responsive, full-stack e-commerce web application built with a PHP backend, Supabase (PostgreSQL) database, and vanilla HTML5/CSS3/JavaScript frontend. It features a complete customer shopping experience, customer account portal, and an administrative management dashboard.

---

## Table of Contents

- [Features](#features)
  - [Customer Storefront](#customer-storefront)
  - [Customer Account Portal](#customer-account-portal)
  - [Admin Dashboard](#admin-dashboard)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Environment Configuration](#environment-configuration)
  - [Database Setup](#database-setup)
  - [Running with Docker](#running-with-docker)
  - [Running with PHP Built-in Server / XAMPP / Apache](#running-with-php-built-in-server--xampp--apache)
- [API Overview](#api-overview)
- [Security Features](#security-features)
- [Deployment](#deployment)
- [License](#license)

---

## Features

### Customer Storefront
- **Home & Catalog Browsing**: Featured collections, promotional banners, and category browsing.
- **Product Search & Filtering**: Real-time filtering by category, price, and search keywords.
- **Product Details**: High-resolution image galleries, pricing, inventory stock status, and product specifications.
- **Shopping Cart**: Dynamic cart management (item quantities, subtotal calculation, persistent cart).
- **Checkout Flow**: Multi-step checkout with address details, order preview, and order placement.
- **Order Confirmation**: Instant order summary and tracking confirmation upon purchase.

### Customer Account Portal
- **Authentication**: User registration, login, logout, and password management.
- **Order History**: View all previous orders with status indicators.
- **Order Tracking & Details**: Granular item breakdown, shipping address, and tracking status.
- **Printable Invoices**: Clean, printable HTML invoice generator for completed orders.
- **Wishlist**: Save favorite items for future purchases.
- **Profile Management**: Update profile info and account credentials.

### Admin Dashboard
- **Admin Authentication**: Dedicated administrative login and session verification.
- **Product Management (CRUD)**: Add, edit, remove, and manage catalog items, pricing, inventory, and categories.
- **Image Uploads**: Upload product images directly to Supabase Storage / media repository.
- **Order Management**: Monitor customer orders, update order status (`pending`, `processing`, `shipped`, `delivered`, `cancelled`).
- **Payment Status Management**: Track and update payment states (`pending`, `paid`, `refunded`).
- **Customer Overview**: Review registered customer details and transaction histories.

---

## Tech Stack

- **Backend**: PHP 8.2+ (Modular API, PDO with PostgreSQL / Supabase, Strict Types)
- **Frontend**: Vanilla JavaScript (ES6+), HTML5, Custom Responsive CSS3
- **Database**: PostgreSQL (hosted on [Supabase](https://supabase.com))
- **Web Server / Routing**: Apache with `mod_rewrite` / Custom PHP Universal Front-Controller (`index.php`)
- **Containerization**: Docker (Apache + PHP 8.2 runtime)

---

## Project Structure

```text
velora-ecommerce/
├── backend/
│   ├── api/                     # RESTful API endpoints
│   │   ├── admin-login.php      # Admin authentication
│   │   ├── admin-orders.php     # Admin order management
│   │   ├── admin-products.php   # Admin product catalog CRUD
│   │   ├── admin-upload-image.php # Product image upload handler
│   │   ├── create-order.php     # Customer order creation
│   │   ├── health.php           # Service health check & DB status
│   │   ├── login.php            # Customer login
│   │   ├── orders.php           # Customer orders API
│   │   ├── products.php         # Public product catalog API
│   │   ├── register.php         # Customer registration
│   │   └── ...                  # Additional API modules
│   ├── config/
│   │   ├── database.php         # Supabase PostgreSQL PDO connection
│   │   └── .htaccess            # Config directory security rules
│   └── includes/
│       ├── admin-auth.php       # Admin authentication middleware
│       ├── auth.php             # User session verification
│       ├── bootstrap.php        # Global initialization & session setup
│       ├── csrf.php             # Cross-Site Request Forgery protection
│       ├── env-loader.php       # .env file parser & environment injector
│       └── response.php         # Standard JSON response helpers
├── frontend/
│   ├── css/
│   │   ├── style.css            # Storefront styling
│   │   └── admin.css            # Admin portal styling
│   ├── js/
│   │   ├── script.js            # Storefront dynamic logic & cart handling
│   │   ├── admin.js             # Admin dashboard SPA functionality
│   │   ├── my-orders.js         # Order tracking logic
│   │   ├── invoice.js           # Invoice rendering
│   │   └── ...                  # Page-specific JS controllers
│   ├── images/                  # Static assets & placeholders
│   ├── index.html               # Storefront homepage
│   ├── products.html            # Product catalog page
│   ├── product-details.html     # Single product view
│   ├── cart.html                # Cart view
│   ├── checkout.html            # Checkout view
│   ├── admin.html               # Admin control panel
│   └── ...                      # Additional view templates
├── .env.example                 # Sample environment configuration
├── .gitignore                   # Git exclusion rules
├── .htaccess                    # Root Apache rewrite and URL routing rules
├── Dockerfile                   # Docker container definition
├── index.php                    # Universal root router and fallback handler
└── README.md                    # Project documentation
```

---

## Getting Started

### Prerequisites

- **PHP 8.2 or later** with the following extensions:
  - `pdo`
  - `pdo_pgsql`
  - `fileinfo`
  - `curl`
  - `session`
- **PostgreSQL** database (a free [Supabase](https://supabase.com) project is recommended).
- **Docker** (optional, for containerized local development or deployment).

---

### Environment Configuration

1. Copy the sample environment file to create your `.env`:
   ```bash
   cp .env.example .env
   ```

2. Open `.env` and fill in your Supabase / PostgreSQL database credentials:
   ```ini
   # PostgreSQL / Supabase Database Connection
   DB_HOST=db.xxxxxxxxxxxxxxxxxxxx.supabase.co
   DB_PORT=5432
   DB_NAME=postgres
   DB_USER=postgres
   DB_PASSWORD=your-database-password

   # Supabase Storage & API Keys
   SUPABASE_URL=https://xxxxxxxxxxxxxxxxxxxx.supabase.co
   SUPABASE_PUBLISHABLE_KEY=your-supabase-publishable-key
   SUPABASE_SECRET_KEY=your-supabase-secret-service-role-key
   ```

---

### Database Setup

1. In your PostgreSQL / Supabase SQL Editor, verify that your tables are created (e.g., `users`, `products`, `orders`, `order_items`, `admins`).
2. Test database connectivity by requesting the health check endpoint:
   ```
   GET /health
   ```
   A successful response will return:
   ```json
   {
     "success": true,
     "php": "8.2.x",
     "database_connected": true,
     "database": "postgres"
   }
   ```

---

### Running with Docker

You can build and run the application locally using Docker:

1. **Build the Docker Image**:
   ```bash
   docker build -t velora-ecommerce .
   ```

2. **Run the Container**:
   ```bash
   docker run -d -p 8080:80 --env-file .env --name velora velora-ecommerce
   ```

3. Open your browser and navigate to `http://localhost:8080`.

---

### Running with PHP Built-in Server / XAMPP / Apache

#### Option A: PHP Built-in Web Server (Development)
You can quickly run the app from the project root:
```bash
php -S localhost:8000 index.php
```
Access the application at `http://localhost:8000`.

#### Option B: Apache / XAMPP
1. Place or link the `velora-ecommerce` folder into your Apache document root (e.g., `htdocs/` for XAMPP).
2. Ensure `mod_rewrite` is enabled in `httpd.conf`:
   ```apache
   LoadModule rewrite_module modules/mod_rewrite.so
   ```
3. Ensure `.htaccess` overrides are permitted (`AllowOverride All`).
4. Access the site at `http://localhost/velora-ecommerce` or configure a VirtualHost.

---

## API Overview

The backend exposes a JSON REST API under `/api/` or `/backend/api/`:

| Endpoint | Method | Access | Description |
|---|---|---|---|
| `/health` | `GET` | Public | System & database health status |
| `/api/products.php` | `GET` | Public | List products with optional search & filter |
| `/api/product-details.php` | `GET` | Public | Get single product by `id` |
| `/api/register.php` | `POST` | Public | Register a new customer account |
| `/api/login.php` | `POST` | Public | Authenticate customer & start session |
| `/api/logout.php` | `POST` | Customer | Destroy customer session |
| `/api/session.php` | `GET` | Public | Check current customer session state |
| `/api/profile.php` | `GET`, `PUT` | Customer | Retrieve or update customer profile |
| `/api/create-order.php` | `POST` | Customer | Place a new customer order |
| `/api/orders.php` | `GET` | Customer | List customer orders |
| `/api/order-details.php` | `GET` | Customer | Get full order details & line items |
| `/api/admin-login.php` | `POST` | Public | Authenticate administrator |
| `/api/admin-session.php` | `GET` | Public | Verify active admin session |
| `/api/admin-products.php` | `GET`, `POST`, `PUT`, `DELETE` | Admin | Full product inventory CRUD |
| `/api/admin-upload-image.php` | `POST` | Admin | Upload product images |
| `/api/admin-orders.php` | `GET` | Admin | Fetch all orders across customers |
| `/api/admin-update-order-status.php` | `POST` | Admin | Update processing status of an order |
| `/api/admin-update-payment-status.php` | `POST` | Admin | Update payment status of an order |

---

## Security Features

- **Prepared Statements**: All database interactions use PDO prepared statements with parameter binding to prevent SQL injection.
- **CSRF Protection**: State-changing requests are guarded by CSRF tokens generated per session.
- **Secure Cookie Settings**: Session cookies configured with `HttpOnly`, `SameSite=Lax`, and dynamic `Secure` flags when served over HTTPS.
- **Input Validation & Sanitization**: Strict input casting, email validation, and positive integer checks on route parameters.
- **Directory Access Control**: Apache configuration prevents direct HTTP access to configuration directories and sensitive files.

---

## Deployment

The repository includes a production-ready `Dockerfile` and `index.php` front-controller configured for containerized hosting services such as:
- **Render** (Web Service with Docker environment)
- **Railway**
- **AWS ECS / DigitalOcean App Platform**
- **Traditional LAMP/LEMP VPS**

When deploying to platforms like Render or Railway:
1. Connect the Git repository.
2. Choose **Docker** as the environment runtime.
3. Configure environment variables (`DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_SECRET_KEY`) in the platform dashboard.
4. Set the health check path to `/health`.

---

## License

This project is licensed under the [MIT License](LICENSE) (or proprietary to the Velora E-Commerce team).
