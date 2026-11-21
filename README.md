# Laravel POS Customer and User Management

> A Laravel POS application continued from TFA5–TFA7. It uses resource routes for customer and user account management, Laravel Breeze for authentication, and auth middleware to protect account pages.

## Features

- **Home (`/`)** — introduces the POS application and links to its main sections.
- **About (`/about`)** — describes the project.
- **Login and Registration (`/login`, `/register`)** — authentication pages scaffolded with Laravel Breeze.
- **Customer Accounts (`/customers`)** — displays customer records; login is required.
- **Create Customer (`/customers/create`)** — adds a customer after validating the required full name and valid email address.
- **Edit Customer (`/customers/{customer}/edit`)** — displays existing customer details and updates the record when submitted.
- **User Accounts (`/users`)** — displays staff accounts, avatars, and a placeholder for users without an avatar; login is required.
- **Create User (`/users/create`)** — adds a user after validating the required, unique username and full name.
- **Edit User (`/users/{user}/edit`)** — displays existing user details and allows an optional avatar upload.
- **Avatar processing** — validates uploaded images, prepares a display-ready image with Intervention Image, and stores it on Laravel's public disk.
- **Profile (`/profile`)** — lets the signed-in user update their own account details and password.

## Requirements

- PHP 8.2 or later
- Composer
- Node.js and npm
- SQLite, or another database configured in `.env`
- PHP GD extension for image processing

## Installation and Setup

1. **Open the project folder** in a terminal.

2. **Install PHP dependencies:**

   ```bash
   composer install
   ```

3. **Create the environment file if it does not exist.**

   Windows PowerShell:

   ```powershell
   Copy-Item .env.example .env
   ```

   macOS/Linux:

   ```bash
   cp .env.example .env
   ```

4. **Generate the application key:**

   ```bash
   php artisan key:generate
   ```

5. **Configure the database** in `.env`. For SQLite, set:

   ```env
   DB_CONNECTION=sqlite
   DB_DATABASE=database/database.sqlite
   ```

   Make sure the SQLite database file exists before running migrations.

6. **Install and build frontend dependencies:**

   ```bash
   npm install
   npm run build
   ```

   In PowerShell, use `npm.cmd install` and `npm.cmd run build` if the script execution policy blocks `npm`.

7. **Create the database tables:**

   ```bash
   php artisan migrate
   ```

   Avoid `php artisan migrate:fresh` if you need to keep existing local data.

8. **Create the public storage link** for uploaded avatars:

   ```bash
   php artisan storage:link
   ```

9. **Start the local server:**

   ```bash
   php artisan serve
   ```

10. **Open the application** at the URL shown in the terminal, usually `http://127.0.0.1:8000`.

## Application Pages

| URL | Page |
|---|---|
| `/` | Home |
| `/about` | About |
| `/login` | Login |
| `/register` | Registration |
| `/customers` | Customer Accounts — login required |
| `/customers/create` | Create Customer — login required |
| `/customers/{customer}/edit` | Edit Customer — login required |
| `/users` | User Accounts — login required |
| `/users/create` | Create User — login required |
| `/users/{user}/edit` | Edit User — login required |
| `/profile` | Signed-in user's profile |

Customer and user routes use `Route::resource()` and omit the unused `show` and `destroy` actions. The Breeze authentication routes are loaded from `routes/auth.php`.

## Database Tables

### `customers`

Stores customer names, email addresses, phone numbers, and timestamps. A customer's full name and valid email address are required. The phone number is optional.

### `users`

Stores usernames, full names, email addresses, password hashes, avatar paths, and timestamps. Usernames must be unique. Avatar paths point to prepared images stored on the public disk.

## Avatar Upload

The user edit form accepts an optional image no larger than 2 MB. Intervention Image prepares the uploaded image for display. Laravel stores the prepared image on the public disk, and the application saves its path in the `avatar` column.

Users without an uploaded avatar are shown a placeholder image.

## Authentication and Access Control

Customer and user account routes are protected by Laravel's `auth` middleware. When a guest visits `/customers` or `/users`, Laravel redirects them to the login page. After logging in, the user can access those pages.

The profile page allows the signed-in user to update their own full name, username, and email. It also has a separate password-change form that requires the current password and confirmation of the new password.

To view the resource routes and their middleware, run:

```bash
php artisan route:list --path=customers -v
php artisan route:list --path=users -v
```
