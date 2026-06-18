# ISMO Project Management System

A sleek, modern, mobile-responsive project and task management system built using a **Next.js** frontend, an **Express + TypeScript** backend, and **PostgreSQL** managed with **Prisma ORM**.

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Next.js 16 (App Router), Tailwind CSS v4, Axios, Lucide Icons |
| **Backend** | Node.js, Express, TypeScript, Zod, JWT, Bcrypt.js |
| **Database** | PostgreSQL via Supabase, Prisma ORM |
| **Containerization** | Docker, Docker Compose (optional) |

## Project Documentation

For judges and reviewers, please refer to the following architectural and API documentation files located in the root of the project:

- **Database Schema & ER Diagram**: Available as [Markdown (.md)](./Database_Schema.md) or [Word Document (.docx)](./Database_Schema.docx) - Contains the ER diagram mapping the `User`, `Project`, and `Task` relationships, as well as the Prisma models configuration.
- **API Documentation**: Available as [Markdown (.md)](./API_Documentation.md) or [Word Document (.docx)](./API_Documentation.docx) - Contains a comprehensive guide to all available backend endpoints, their required payloads, and expected responses.

---

## Prerequisites

Before starting, make sure you have these installed:
- [Node.js](https://nodejs.org/) v18 or higher
- [Git](https://git-scm.com/)
- A free [Supabase](https://supabase.com/) account

---

## Quickstart for New Developers

### Step 1 — Clone the Repository
```bash
git clone <your-repo-url>
cd "ISMO Project"
```

**Important:** This project uses Git submodules for the frontend and backend. After cloning the main repository, you **must** pull the submodules to get the complete code. Run the following command:

```bash
git submodule update --init --recursive
```

Please take a moment to observe the project structure outlined at the bottom of this README to understand how the submodules are integrated before proceeding.

### Step 2 — Create Your Supabase Database

> **Every developer needs their own Supabase project** (each has their own isolated database).

1. Go to [supabase.com](https://supabase.com/) and log in.
2. Click **New Project** → name it anything (e.g. `ismo-pm-dev`) → set a strong database password → click **Create**.
3. Wait ~1 minute for the database to provision.
4. Go to **Project Settings** (gear icon) → **Database** → scroll to **Connection string**.
5. Select **URI** tab and copy the connection string. It looks like:
   ```
   postgresql://postgres.[PROJECT_REF]:[YOUR_PASSWORD]@aws-0-[REGION].pooler.supabase.com:6543/postgres
   ```
6. **Save this URL** — you will need it in the next step.

---

### Step 3 — Configure the Backend

```bash
cd backend
```

Copy the example env file and fill in your values:
```bash
# Windows (PowerShell)
Copy-Item .env.example .env

# macOS / Linux
cp .env.example .env
```

Open `.env` and update it:
```env
DATABASE_URL="postgresql://postgres.[YOUR_REF]:[YOUR_PASSWORD]@aws-0-[REGION].pooler.supabase.com:6543/postgres"
PORT=5000
JWT_SECRET="any_long_random_string_of_your_choice_min_32_chars"
FRONTEND_URL="http://localhost:3000"
```

---

### Step 4 — Install Dependencies & Migrate the Database

```bash
npm install
```

Now run the database migration. This command reads the Prisma schema and **automatically creates all the tables** (`User`, `Project`, `Task`) in your Supabase database:

```bash
npx prisma migrate deploy
```

> **What this does:** Prisma reads the migration files in `prisma/migrations/` and applies them to your Supabase PostgreSQL database. This is the correct command for a fresh database — it is non-interactive and requires no prompts.

You should see output like:
```
Applying migration `20240101_init`
All migrations have been applied.
```

Finally, generate the local Prisma client:
```bash
npx prisma generate
```

---

### Step 5 — Start the Backend Server

```bash
npm run dev
```

You should see:
```
[server]: Server is running at http://localhost:5000
[server]: Accepting requests from http://localhost:3000
```

---

### Step 6 — Configure & Start the Frontend

Open a new terminal window:

```bash
cd frontend
npm install
npm run dev
```

The frontend runs at **http://localhost:3000**.

---

### Step 7 — Verify It Works

1. Open your browser at `http://localhost:3000`
2. Click **Create an account** and register with any email/password.
3. Create a project and add tasks.
4. To confirm data is in your Supabase: go to your Supabase dashboard → **Table Editor** → you'll see your rows in the `User`, `Project`, and `Task` tables.

---

## Developer Cheat Sheet

| Task | Command | Directory |
|---|---|---|
| Start backend | `npm run dev` | `backend/` |
| Start frontend | `npm run dev` | `frontend/` |
| Apply DB migrations | `npx prisma migrate deploy` | `backend/` |
| Create a new migration (schema change) | `npx prisma migrate dev --name <description>` | `backend/` |
| Regenerate Prisma client | `npx prisma generate` | `backend/` |
| Open Prisma Studio (visual DB GUI) | `npx prisma studio` | `backend/` |
| Build frontend for production | `npm run build` | `frontend/` |

---

## 🐳 Running with Docker (The Easiest Way)

If another developer wants to run this project without setting up Node.js, Supabase, or any environment variables, they can use Docker. 

Docker will automatically spin up a local PostgreSQL database, build the backend, and build the frontend Next.js app in one go.

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) must be installed and running.

### Step 1 — Start the Stack
Open a terminal at the root of the cloned repository and run:
```bash
docker-compose up --build
```
*(This will download the necessary images and start the database, backend, and frontend. Wait until you see the logs stabilize).*

### Step 2 — Run Database Migrations
Because Docker creates a completely fresh, empty database, you must create the tables before logging in. 

Open a **new, separate terminal window** and run this command to build the tables inside the running backend container:
```bash
docker exec -it ismo_pm_backend npx prisma migrate deploy
```

### Step 3 — Use the App!
Everything is now running. Open your browser:
| Service | URL |
|---|---|
| **Frontend Web App** | http://localhost:3000 |
| Backend API | http://localhost:5000 |
| Local Postgres DB | localhost:5432 |

### Stopping the App
To stop the application, go to the terminal running Docker and press `Ctrl+C`, or run:
```bash
docker-compose down
```
*(Note: Your data is safely stored in a local Docker volume. It will still be there the next time you run `docker-compose up`. If you want to permanently delete the database and start fresh, run `docker-compose down -v`)*.

---

## Project Structure

```
ISMO Project/
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma        # Database models
│   │   └── migrations/          # Migration history
│   ├── src/
│   │   ├── controllers/         # Route logic
│   │   ├── middleware/          # JWT auth, rate limiter
│   │   ├── routes/              # Express route definitions
│   │   └── utils/               # DB client, validation schemas
│   ├── .env.example             # Environment variable template
│   └── prisma.config.ts         # Prisma configuration
└── frontend/
    └── src/
        ├── app/
        │   ├── (auth)/          # Login, Register pages
        │   └── (dashboard)/     # Dashboard, Projects, Tasks pages
        ├── context/             # AuthContext (JWT session state)
        └── utils/               # Axios API client
```
