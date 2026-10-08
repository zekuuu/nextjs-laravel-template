# Next.js Laravel Template

![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=nextdotjs)
![Laravel](https://img.shields.io/badge/Laravel-13-ff2d20?logo=laravel&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-ready-3178c6?logo=typescript&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-4-38bdf8?logo=tailwindcss&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green)

Full-stack Next.js and Laravel template with TypeScript, TailwindCSS, Axios, shadcn/ui-ready configuration, and Laravel Sanctum preparation.

## Tech Stack

- Next.js App Router
- TypeScript
- TailwindCSS
- shadcn/ui-ready structure
- Axios
- Laravel
- Laravel Sanctum
- npm for frontend package management
- Generic database placeholders so you can choose MySQL, PostgreSQL, SQLite, SQL Server, or another Laravel-supported driver later

## Folder Structure

```txt
nextjs-laravel-template/
├── client/
│   ├── src/
│   │   ├── app/
│   │   │   ├── (auth)/
│   │   │   ├── (protected)/
│   │   ├── components/
│   │   ├── lib/
│   │   └── proxy.ts
│   ├── public/
│   ├── .env.example
│   ├── components.json
│   ├── package.json
│   └── tsconfig.json
├── server/
│   ├── app/
│   │   ├── Http/
│   │   │   ├── Controllers/
│   │   │   │   └── Api/
│   │   │   ├── Middleware/
│   │   │   ├── Requests/
│   │   │   └── Responses/
│   │   ├── Models/
│   │   ├── Repositories/
│   │   └── Services/
│   ├── bootstrap/
│   ├── config/
│   ├── database/
│   ├── routes/
│   │   ├── api.php
│   │   └── web.php
│   ├── storage/
│   ├── tests/
│   ├── .env.example
│   └── composer.json
├── .gitignore
├── LICENSE
├── package.json
└── README.md
```

## Use This Template Without Git History

The cleanest way to start a new project is to use GitHub's **Use this template** button. Your new repository starts with its own Git history and does not keep this template repository as `origin`.

You can also download the repository as a ZIP file, extract it, then initialize your own Git repository:

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main
```

If you cloned this template directly, remove the existing Git history before creating your own first commit:

```bash
git clone https://github.com/zekuuu/nextjs-laravel-template.git your-project-name
cd your-project-name
rm -rf .git
git init
git add .
git commit -m "Initial commit"
```

On Windows PowerShell:

```powershell
Remove-Item -Recurse -Force .git
git init
git add .
git commit -m "Initial commit"
```

## Frontend Setup

```bash
cd client
npm install
npm run dev
```

The frontend runs at:

```txt
http://localhost:3000
```

Important frontend files:

- `client/src/app/page.tsx` for the homepage placeholder
- `client/src/app/(auth)/login/page.tsx` and `client/src/app/(auth)/register/page.tsx` for future auth screens
- `client/src/app/(protected)/dashboard/page.tsx` and `client/src/app/(protected)/settings/page.tsx` for future protected screens
- `client/src/components/Navbar.tsx` and `client/src/components/Footer.tsx` for layout UI
- `client/src/components/LoadingSkeleton.tsx` for reusable loading states
- `client/src/components/ui/Icons.tsx` for typed lucide-react icons
- `client/src/lib/api.ts` for the Axios API client, currently pointed at the versioned API base URL
- `client/src/proxy.ts` for the Next.js request proxy, formerly called middleware

## Backend Setup

```bash
cd server
composer install
cp .env.example .env
php artisan key:generate
php artisan serve
```

The backend runs at:

```txt
http://localhost:8000
```

Important backend files:

- `server/routes/api.php` for versioned API routes under `/api/v1`
- `server/app/Http/Controllers/Api/AuthController.php` for future auth endpoints
- `server/app/Http/Controllers/Api/UserController.php` for future user endpoints
- `server/app/Http/Responses/ApiResponse.php` for consistent JSON response formatting
- `server/.env.example` for app, database, CORS, session, and Sanctum examples

## Root Development Setup

Install the root dependency:

```bash
npm install
```

Run the frontend and backend together from the root folder:

```bash
npm run dev
```

This runs:

```bash
cd client && npm run dev
```

and:

```bash
cd server && php artisan serve --host=127.0.0.1 --port=8000
```

The root command uses colored process labels through `concurrently`.

## Environment Variables

Frontend:

```bash
cd client
cp .env.example .env.local
```

Backend:

```bash
cd server
cp .env.example .env
php artisan key:generate
```

The frontend uses:

```env
NEXT_PUBLIC_API_URL=http://localhost:8000/api/v1
NEXT_PUBLIC_BACKEND_URL=http://localhost:8000
```

`NEXT_PUBLIC_API_URL` points Axios to Laravel's versioned API routes. In this template, the API version is `v1`, so frontend requests are based on `http://localhost:8000/api/v1`. `NEXT_PUBLIC_BACKEND_URL` is useful for non-API endpoints such as Sanctum's CSRF cookie route.

## Laravel Sanctum Notes

Laravel Sanctum provides lightweight authentication for SPAs and APIs. This template is auth-ready, but intentionally keeps auth logic as placeholders so you can add only the behavior your app needs.

For separate local development:

```txt
Frontend: http://localhost:3000
Backend:  http://localhost:8000
```

Sanctum SPA authentication commonly uses:

```txt
/sanctum/csrf-cookie
```

before login requests. When you implement cookie-based SPA auth, configure these carefully:

- `SANCTUM_STATEFUL_DOMAINS=localhost:3000,127.0.0.1:3000`
- `SESSION_DOMAIN=localhost`
- CORS allowed origins including `http://localhost:3000`
- Axios `withCredentials` when you intentionally use cookie/session auth

This template does not force frontend auth behavior yet. The Axios helper is ready to call Laravel, but it does not automatically fetch the CSRF cookie or attach credentials until you choose that flow.

To remove auth later, delete the auth routes in `server/routes/api.php`, remove the placeholder API auth controllers, remove `laravel/sanctum` from `server/composer.json`, and remove the auth pages under `client/src/app/(auth)`.

## API Route Overview

Prepared placeholder routes are versioned under `/api/v1`:

```txt
GET  /api/v1/health
GET  /api/v1/user
POST /api/v1/register
POST /api/v1/login
POST /api/v1/logout
```

Use these files when adding logic:

- Routes: `server/routes/api.php`
- Auth controller: `server/app/Http/Controllers/Api/AuthController.php`
- User controller: `server/app/Http/Controllers/Api/UserController.php`
- Shared responses: `server/app/Http/Responses/ApiResponse.php`

To change the API version later, update the route prefix in `server/routes/api.php`, then update `NEXT_PUBLIC_API_URL` in `client/.env.example`, your local frontend env file, and `client/src/lib/api.ts`.

## Database Flexibility

The database is intentionally not finalized. The backend `.env.example` uses commented placeholders so you can choose the database later.

Common Laravel-supported choices:

- MySQL: `DB_CONNECTION=mysql`, usually port `3306`
- PostgreSQL: `DB_CONNECTION=pgsql`, usually port `5432`
- SQLite: `DB_CONNECTION=sqlite`, often using a local database file

Do not commit real credentials. Keep local secrets in `.env`.

## Development Workflow

Recommended first run:

```bash
npm install
cd client
npm install
cd ../server
composer install
cp .env.example .env
php artisan key:generate
cd ..
npm run dev
```

Recommended commands:

```bash
npm run dev
cd client && npm run dev
cd client && npm run build
cd server && php artisan serve
cd server && php artisan route:list
```

## Deployment Notes

Frontend deployment options:

- Vercel is the most direct option for the Next.js client.
- Set `NEXT_PUBLIC_API_URL` to your production Laravel API URL, including the API version path such as `/api/v1`.
- Set `NEXT_PUBLIC_BACKEND_URL` to your production Laravel backend URL.

Backend deployment options:

- Laravel Forge
- Render
- Railway
- Fly.io
- VPS or shared hosting that supports Laravel requirements

For production Sanctum SPA auth, update your production CORS origins, stateful domains, session domain, HTTPS settings, and cookie settings. Do not reuse localhost values in production.

## Customization Guide

- Routes and API version prefix: edit `server/routes/api.php`
- Components: edit `client/src/components`
- API calls: edit `client/src/lib/api.ts`
- Auth logic: edit `server/app/Http/Controllers/Api/AuthController.php`
- User logic: edit `server/app/Http/Controllers/Api/UserController.php`
- Database config: edit `server/.env`
- shadcn/ui: use `client/components.json` and add generated components under `client/src/components/ui`

## License

This project is open-sourced under the MIT License.

## Credits

Created by Zean Kurt G. Balboa
