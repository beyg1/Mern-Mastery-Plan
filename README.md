# MERN Mastery Plan — Gold Edition 🚀

Welcome to MERN Mastery Plan! This roadmap is designed for developers who are already proficient in React/Next.js and have experience building full-stack applications, either with headless CMS like Sanity or Full PostgreSQL Database like Supabase.

## 🔧 Gold Path Forward
- Backend: Node.js + Express, Mongoose, MongoDB Atlas, JWT, bcrypt, express-validator, cors, morgan
- Frontend: Next.js (pick Pages or App Router once and stick to it), built-in fetch
- Deployment: Vercel (frontend), Render (backend), MongoDB Atlas (DB)

---

Repository layout (per-phase directories):
- `/phase0-setup`
- `/phase1-express-in-memory`
- `/phase2-mongodb-mongoose`
- `/phase3-next-list-create`
- `/phase4-next-update-delete`
- `/phase5-backend-auth`
- `/phase6-client-auth`
- `/phase7-quality`
- `/phase8-deploy`
- `/phase9-stretch`

Each phase directory is self-contained and may include:
- `/server` for the Express backend (as needed in that phase)
- `/client` for the Next.js frontend (App Router; introduced in Phase 3)
- `.env.sample` files documenting only the variables required for that phase
- Phase-specific instructions (this README serves as the master guide)

## 🧪 Phase 0 — Setup Safety Net (30–60 min, beginner-friendly step-by-step)

Goal: Prepare a frictionless environment so you don’t stall later.

Step-by-step (Phase 0: inside `/phase0-setup`):
1) Install prerequisites
   - Node LTS: https://nodejs.org/en (verify with: `node -v`)
   - pnpm: https://pnpm.io/installation (verify with: `pnpm -v`)
   - Git: https://git-scm.com (verify with: `git --version`)

2) Initialize repo (root folder of your project)
   - Create a new GitHub repo (e.g., MERN Lab) or a local folder.
   - Initialize git in the root:
     - `git init`
     - `git add . && git commit -m "chore: init repo"`
     - `git branch -M main`
     - `git remote add origin <your-remote-url>`
     - `git push -u origin main` (you can push after you add initial files too)

3) Server scaffold (minimal; inside `/phase0-setup/server`)
   - Create `/phase0-setup/server/package.json`:
     ```
     {
       "name": "server",
       "version": "1.0.0",
       "type": "module",
       "scripts": {
         "dev": "node index.js",
         "start": "node index.js"
       }
     }
     ```
   - Create `/phase0-setup/server/index.js`:
     ```
     console.log("Server running. Hello from Express placeholder.");
     ```
   - Create `/phase0-setup/server/.env.sample`:
     ```
     PORT=5000
     ```
   - Note: Do NOT commit real `.env`. Only commit `.env.sample`.

4) Client placeholder for later (inside `/phase0-setup/client`)
   - Create `/phase0-setup/client/.env.sample`:
     ```
     NEXT_PUBLIC_API_URL=http://localhost:5000
     ```
   - The actual Next.js app will be created in Phase 3.

5) How to run per phase
   - Backend (Phase 0 placeholder):
     - `pnpm -C ./phase0-setup/server dev`
     - `pnpm -C ./phase0-setup/server start`
   - Frontend will be set up in Phase 3.

Checklist (Phase 0):
- [ ] Install Node LTS, pnpm, and Git  
- [ ] Create `/phase0-setup/server` and `/phase0-setup/client` with `.env.sample` files  
- [ ] Verify you can run `pnpm -C ./phase0-setup/server dev` (prints “Hello”)

Success Criteria (Phase 0):
- [ ] Local commands run without errors  
- [ ] You can push to GitHub

Notes:
- Keep secrets in `.env` (don’t commit them). Document required variables in `.env.sample`.
- MongoDB and JWT are not required in Phase 0. They are introduced in later phases.

---

## 🟦 Phase 1 — Express API (In-Memory) (60–90 min)

Goal: A simple REST API for tasks backed by an in-memory array. No DB yet.

Install (inside `/phase1-express-in-memory/server`):
- [ ] `pnpm add express cors morgan`
- [ ] `pnpm add -D nodemon`
- [ ] Update scripts in `package.json`:
  ```
  {
    "scripts": {
      "dev": "nodemon index.js",
      "start": "node index.js"
    }
  }
  ```

Implement routes (in `index.js` or split later):
- [ ] `GET /api/tasks` — list all  
- [ ] `GET /api/tasks/:id` — get one  
- [ ] `POST /api/tasks` — create  
- [ ] `PUT /api/tasks/:id` — update  
- [ ] `DELETE /api/tasks/:id` — remove

Middleware:
- [ ] `cors({ origin: "http://localhost:3000" })`  
- [ ] `morgan("dev")`  
- [ ] `express.json()` for JSON body parsing  
- [ ] 404 handler and centralized error handler

How to test quickly with curl:
- GET: `curl http://localhost:5000/api/tasks`
- POST: `curl -X POST -H "Content-Type: application/json" -d '{"title":"Test"}' http://localhost:5000/api/tasks`
- PUT: `curl -X PUT -H "Content-Type: application/json" -d '{"title":"Updated"}' http://localhost:5000/api/tasks/<id>`
- DELETE: `curl -X DELETE http://localhost:5000/api/tasks/<id>`

Run:
- `pnpm -C ./phase1-express-in-memory/server dev`

Success Criteria:
- [ ] All CRUD operations work via Postman/curl  
- [ ] Proper status codes (201 create, 404 not found, 400 bad request)  
- [ ] Automatic restarts with nodemon

STOP and commit. You now understand backend request/response flows without DB complexity.

---

## 🟩 Phase 2 — MongoDB + Mongoose (90–120 min)

Goal: Replace in-memory with MongoDB Atlas and Mongoose model.

Install (inside `/phase2-mongodb-mongoose/server`):
- [ ] `pnpm add mongoose express-validator`

Connect:
- [ ] Create `/phase2-mongodb-mongoose/server/.env` with `MONGODB_URI` and `PORT` (e.g., 5000)
- [ ] On server start, connect to Atlas and log success/failure
- [ ] Handle connection errors and exit or retry appropriately

Task Model:
- [ ] Fields: `title` (required), `description` (optional), `completed` (default false), `dueDate` (optional)  
- [ ] `timestamps: true`

Validation:
- [ ] `express-validator` on POST/PUT: e.g., `title` must be a non-empty string

Controllers (replace array CRUD with Mongoose):
- [ ] `Task.find()`  
- [ ] `Task.findById()`  
- [ ] `Task.create()`  
- [ ] `Task.findByIdAndUpdate()`  
- [ ] `Task.findByIdAndDelete()`

Errors:
- [ ] Consistent error shape: `{ message, details? }`
- [ ] Return `400` for validation issues or invalid ObjectId

Success Criteria:
- [ ] Data persists across server restarts  
- [ ] Validation errors are clear and return 400

Tips (beginner-friendly):
- If you see “Topology description” or timeout, check:
  - Atlas IP allowlist
  - Correct username/password in connection string
  - Proper database name in URI
- For invalid `:id`, use Mongoose `isValidObjectId` to respond with 400 instead of 500.

---

## 🟨 Phase 3 — Next.js Client: List + Create (90–120 min)

Goal: Minimal UI (App Router) to list tasks and create tasks.

Setup (inside `/phase3-next-list-create/client`):
- [ ] Create Next.js app (App Router):
  - `pnpm create next-app@latest .`
  - Choose TypeScript if you want; keep defaults simple.
- [ ] Create `.env.local` with:
  ```
  NEXT_PUBLIC_API_URL=http://localhost:5000
  ```
- [ ] Choose one data fetching strategy and stick to it initially:
  - App Router: server components fetch or client components with `useEffect`
  - Beginner-friendly: client component with `useEffect` + `fetch`

Features:
- [ ] Display task list  
- [ ] Simple form to create a task (title only)  
- [ ] Show loading and error states (plain text is fine)

Fetch example (client component):
```
const base = process.env.NEXT_PUBLIC_API_URL;

useEffect(() => {
  fetch(base + "/api/tasks")
    .then(r => r.json())
    .then(setTasks)
    .catch(setError);
}, []);

async function createTask(title) {
  const res = await fetch(base + "/api/tasks", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ title })
  });
  if (!res.ok) throw new Error("Failed to create");
  await refetchTasks();
}
```

Run:
- `pnpm -C ./phase3-next-list-create/client dev`
- Open `http://localhost:3000`

Success Criteria:
- [ ] Creating a task updates the list  
- [ ] Errors are visible in the UI

---

## 🟪 Phase 4 — Next.js Client: Update + Delete (60–90 min)

Goal: Complete CRUD from the UI (App Router).

Context: continue in `/phase4-next-update-delete/client` (copy from Phase 3 or start fresh as preferred).

Features:
- [ ] Edit task title (inline input toggle or separate page/modal)  
- [ ] Delete with a `confirm()` prompt  
- [ ] Refresh list after success (or refetch call)
  - Tip: keep a reusable `fetchTasks()` function or use SWR/React Query later

Run:
- `pnpm -C ./phase4-next-update-delete/client dev`

Success Criteria:
- [ ] Update and delete work reliably  
- [ ] UI reflects latest server state

---

## 🔐 Phase 5 — Backend Auth (JWT) (120–180 min)

Goal: Protect routes via JWT, scope tasks per user.

Install (inside `/phase5-backend-auth/server`):
- [ ] `pnpm add bcrypt jsonwebtoken`

User model:
- [ ] Fields: `email` (unique), `password` (hashed)

Auth routes:
- [ ] `POST /auth/register` — hash with bcrypt, create user  
- [ ] `POST /auth/login` — verify password, sign JWT with user id

Middleware:
- [ ] `auth` middleware to verify `Authorization: Bearer <token>`  
- [ ] Attach `req.user` with `{ id }`

Task scoping:
- [ ] Add `userId` to Task model  
- [ ] All task queries filter by `userId`

JWT tips:
- Store `JWT_SECRET` in `/phase5-backend-auth/server/.env`  
- Sign with short payload (e.g., `{ id: user._id }`)  
- Return 401 for missing/invalid token

Success Criteria:
- [ ] Unauthenticated requests to tasks return 401  
- [ ] Each user sees only their tasks

---

## 🔑 Phase 6 — Client Auth Integration (90–120 min)

Goal: Persist login and call API with token.

Context: `/phase6-client-auth/client` (App Router).

State:
- [ ] Simple auth context or top-level state + `localStorage`  
- [ ] On login success, store token  
- [ ] Include `Authorization: Bearer <token>` in fetch

Routing:
- [ ] Redirect unauthenticated users to `/login` for protected pages  
- [ ] Optional: `/me` endpoint or decode token to show email

Fetch with token example:
```
const token = typeof window !== "undefined" ? localStorage.getItem("token") : null;

fetch(base + "/api/tasks", {
  headers: {
    Authorization: `Bearer ${token}`
  }
});
```

Run:
- `pnpm -C ./phase6-client-auth/client dev`

Success Criteria:
- [ ] Can register, login, and manage your own tasks

---

## 🧹 Phase 7 — Quality Pass (90–120 min)

Goal: Predictability and maintainability.

Structure (inside `/phase7-quality/server`):
- [ ] Server folders: `/models`, `/routes`, `/controllers`, `/middleware`, `/utils`  
- [ ] Add `/health` route

Linting & Tests:
- [ ] `pnpm add -D jest supertest` (inside `/phase7-quality/server`)
- [ ] One unit test (pure function)  
- [ ] One integration test (e.g., `POST /tasks` happy path)

Scripts (server `package.json`):
```
"test": "jest"
```

Run:
- `pnpm -C ./phase7-quality/server test`

Success Criteria:
- [ ] Tests run and pass locally  
- [ ] Lint step integrated into dev flow

---

## 🚀 Phase 8 — Deployment (120–180 min)

Goal: Ship the app live.

Backend (from the relevant server inside your chosen phase, e.g., `/phase8-deploy/server`):
- [ ] Deploy to Render (or similar)  
- [ ] Set `MONGODB_URI`, `JWT_SECRET` in Render  
- [ ] CORS for your Vercel domain

Frontend (from `/phase8-deploy/client`):
- [ ] Deploy to Vercel  
- [ ] Set `NEXT_PUBLIC_API_URL` to your Render URL

Database:
- [ ] Use MongoDB Atlas  
- [ ] Ensure IP allowlist allows Render (or allow from anywhere during dev)

Success Criteria:
- [ ] Register, login, CRUD tasks on production URLs  
- [ ] README updated with deploy URLs and run instructions

Common deployment pitfalls:
- Wrong env var names  
- Not enabling CORS for the production domain  
- Forgetting to use production URLs in `NEXT_PUBLIC_API_URL`

---

## 🧗 Phase 9 — Stretch Features (choose 1–2; 60–180 min each)

Options:
- [ ] Pagination and sorting  
- [ ] Search by title  
- [ ] Due dates and reminders  
- [ ] File uploads (Cloudinary/S3)  
- [ ] Role-based auth (admin)  
- [ ] Security: rate limiting, helmet  
- [ ] Caching with Redis  
- [ ] E2E tests (Playwright/Cypress)  
- [ ] Feature-based directory structure on client

Rule: Only pick what aligns with your current needs. No boiling the ocean.

---

## 🗓️ Study Routine to Avoid Overwhelm

- Use 25–50 minute focus blocks with one acceptance criterion (e.g., “POST /tasks returns 201 with created doc”).  
- End each session with:
  - A commit
  - A short note: “Next session: implement PUT /tasks/:id validation”
- Keep a simple `BUGS_TODO.md` in the repo; never keep tasks in your head.  
- Celebrate small wins: getting server to run, first DB insert, first deployed route.

---

## ✅ Quick Sprint Checklists (Copy-Paste Ready)

Sprint 1: Express In-Memory API
- [ ] express, nodemon, cors, morgan installed  
- [ ] CRUD routes implemented  
- [ ] 404 + error middleware  
- [ ] CORS for localhost:3000  
- Commands: `pnpm -C server dev`

Sprint 2: MongoDB & Mongoose
- [ ] Connect to Atlas with MONGODB_URI  
- [ ] Task schema + timestamps  
- [ ] Replace in-memory with Mongoose CRUD  
- [ ] express-validator for create/update  
- [ ] Consistent error shape  
- Commands: `pnpm -C server dev`

Sprint 3: Next.js (List + Create)
- [ ] NEXT_PUBLIC_API_URL set  
- [ ] Render list of tasks  
- [ ] Create task form with loading & error states  
- Commands: `pnpm -C client dev`

Sprint 4: Next.js (Update + Delete)
- [ ] Edit title flow  
- [ ] Delete with confirm  
- [ ] List refreshes after actions  
- Commands: `pnpm -C client dev`

Sprint 5: Backend Auth
- [ ] User model, bcrypt hashing  
- [ ] /auth/register + /auth/login  
- [ ] JWT verify middleware  
- [ ] Tasks scoped by userId  
- Commands: `pnpm -C server dev`

Sprint 6: Client Auth
- [ ] Auth context + localStorage  
- [ ] Login form + redirect  
- [ ] Authorization header added  
- Commands: `pnpm -C client dev`

Sprint 7: Quality
- [ ] Folder structure by responsibility  
- [ ] ESLint configured  
- [ ] Jest + supertest, 1–2 tests  
- [ ] /health route  
- Commands: `pnpm -C server test`

Sprint 8: Deploy
- [ ] Backend on Render (env set)  
- [ ] Frontend on Vercel (env set)  
- [ ] CORS for prod domain  
- [ ] Manual E2E on prod

---
