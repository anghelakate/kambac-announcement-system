# KAMBAC Announcement System

KAMBAC is a server-rendered announcement system built with Node.js, the XianFire/Express project structure, Handlebars-compatible `.xian` views, Sequelize, and MySQL. Admin users can create and manage announcements they originally posted. Staff and Visitors have view-only access to published announcements shared with them, with recipient-owned read/unread notifications.

## Requirements

- Node.js 20 or newer
- npm
- MySQL 8 or a compatible MySQL server
- A database and database user with permission to create or update the application tables

## Configuration

The application reads configuration from process environment variables. Use [.env.example](.env.example) as a variable checklist; the application does not automatically load `.env` files.

Required for a typical deployment:

| Variable | Purpose |
| --- | --- |
| `SESSION_SECRET` | Long random value used to sign sessions |
| `DB_HOST` | MySQL hostname |
| `DB_PORT` | MySQL port, normally `3306` inside a Docker network |
| `DB_NAME` | Existing MySQL database name |
| `DB_USER` | MySQL user |
| `DB_PASSWORD` | MySQL password |
| `PORT` | Optional HTTP port; defaults to `3000` |

Never commit a populated `.env` file or real credentials.

## Install and run

Set the environment variables in your shell, container, or deployment platform, then run:

```powershell
npm ci
npm run migrate
npm run db:check
npm start
```

`npm run migrate` performs additive setup only: it creates missing application tables, adds missing announcement fields, creates the notification table, and ensures email uniqueness when existing data permits it. It does not use destructive `force` or `alter` synchronization.

Open `http://localhost:3000/login` after startup. Successful login and registration lead to the announcement workspace. New registrations receive the `visitors` role by default.

## Docker image

The included `Dockerfile` builds the Node application image:

```powershell
docker build -t kambac-announcement-system .
```

Run the image with port `3000` published and the configuration variables above supplied by your Docker environment. MySQL is an external dependency and must be reachable through `DB_HOST` and `DB_PORT`; this repository does not create or reset a database container or volume.

## Main commands

- `npm start` — start the application
- `npm run dev` — start through nodemon for local development
- `npm run migrate` — perform additive database setup/migration
- `npm run db:check` — verify database connectivity and required schema without changing data
- `npm run route:list` — list application routes

Password reset is currently a placeholder page and does not perform a reset. Account role changes are managed outside the public registration form.
