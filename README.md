Laravel POS: Resource Routing and Authentication
A Laravel POS application continued from TFA5–TFA7. This version uses Laravel resource routes for customer and user account management, and Laravel Breeze for login and registration.
Features
- Home (/) and About (/about) pages.
- Laravel Breeze authentication with login, registration, and logout.
- Customer Accounts (/customers) with resource routes for listing, creating, storing, editing, and updating customer records.
- User Accounts (/users) with resource routes for listing, creating, storing, editing, and updating staff accounts.
- Authentication protection: only logged-in users can access the customer and user account routes.
- Validated forms for customer and staff account records.
- Avatar uploads prepared for display and saved on Laravel’s public storage disk.
- Profile page (/profile) for updating the signed-in user’s details and password.
Requirements
- PHP 8.2 or later
- Composer
- Node.js and npm
- SQLite or another database supported by Laravel
- PHP GD extension for image processing
Installation and Setup
1. Open the project folder in a terminal.
2. Install PHP dependencies:
   composer install
3. Create the environment file if it does not exist.
   Windows PowerShell:
   Copy-Item .env.example .env
   
   macOS/Linux:
   cp .env.example .env
4. Generate the application key:
   php artisan key:generate
5. Configure the database in .env. For SQLite, use:
   DB_CONNECTION=sqlite
   DB_DATABASE=database/database.sqlite
   
   Create database/database.sqlite if it does not exist.
6. Install and build frontend dependencies:
   npm install
   npm run build
   
   In PowerShell, use npm.cmd install and npm.cmd run build if npm is blocked by the script execution policy.
7. Run the migrations:
   php artisan migrate
   
   This creates the application tables and authentication columns. Avoid php artisan migrate:fresh if you need to keep existing local data.
8. Create the public storage link for avatars:
   php artisan storage:link
9. Start the development server:
   php artisan serve
10. Open the URL shown in the terminal, usually http://127.0.0.1:8000.
Application Routes
URL	Access	Page
/	Public	Home
/about	Public	About
/login	Public	Login
/register	Public	Registration
/customers	Login required	Customer Accounts
/customers/create	Login required	Create Customer
/customers/{customer}/edit	Login required	Edit Customer
/users	Login required	User Accounts
/users/create	Login required	Create User
/users/{user}/edit	Login required	Edit User
/profile	Login required	Signed-in user’s profile


The customer and user routes use Route::resource() and omit the unused show and destroy actions. Breeze authentication routes are loaded from routes/auth.php.
Authentication and Access Control
When logged out, visiting /customers or /users redirects to the login page. After logging in, users can access these pages.

To inspect the routes and their middleware, run:
php artisan route:list --path=customers -v
php artisan route:list --path=users -v

Customer and User Validation
Customer forms require a full name and a valid email address.

User forms require a full name and a unique username. User accounts can also upload an optional avatar image no larger than 2 MB.
Avatar Storage
Uploaded avatars are validated as images, prepared for display using Intervention Image, and saved on Laravel’s public disk. The database stores the resulting file path, not the image itself. Users without an avatar see a placeholder image.
Profile
The signed-in user can update their own full name, username, and email from /profile. The page also has a separate password-change form that requires the current password and a confirmed new password.
