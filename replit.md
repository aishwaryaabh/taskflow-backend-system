# Workspace

## Overview

Full-stack Task Manager app — pnpm workspace monorepo using TypeScript. Features JWT auth, role-based access control (user/admin), and full CRUD for tasks.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (ESM bundle)
- **Frontend**: React + Vite + Tailwind CSS
- **Auth**: JWT (jsonwebtoken) + bcryptjs
- **API Docs**: Swagger UI at `/api/api-docs`

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (JWT auth, RBAC, CRUD)
│   │   └── src/
│   │       ├── config/jwt.ts         # JWT secret + expiry config
│   │       ├── middleware/authenticate.ts  # JWT verify + admin guard
│   │       └── routes/
│   │           ├── auth.ts           # /auth/register, /auth/login, /auth/me
│   │           ├── tasks.ts          # /tasks CRUD (user + admin)
│   │           └── admin.ts          # /admin/users (admin only)
│   └── task-manager/       # React + Vite frontend
│       └── src/
│           ├── pages/
│           │   ├── login.tsx         # Login form
│           │   ├── register.tsx      # Registration form
│           │   ├── dashboard.tsx     # Task dashboard (authenticated)
│           │   └── admin.tsx         # Admin panel (admin role only)
│           ├── lib/
│           │   ├── auth-context.tsx  # Auth state management
│           │   └── fetch-interceptor.ts  # JWT header injection
│           └── components/layout/AppLayout.tsx
├── lib/
│   ├── api-spec/openapi.yaml   # OpenAPI 3.1 spec — source of truth
│   ├── api-client-react/       # Generated React Query hooks
│   ├── api-zod/                # Generated Zod schemas
│   └── db/src/schema/
│       ├── users.ts            # Users table (id, name, email, password, role)
│       └── tasks.ts            # Tasks table (id, title, description, status, userId)
├── scripts/
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## API Endpoints

All routes under `/api/v1/`:

### Auth
- `POST /auth/register` — Register new user
- `POST /auth/login` — Login, get JWT
- `GET /auth/me` — Get current user (requires JWT)

### Tasks (JWT required)
- `GET /tasks` — Get tasks (users see own, admins see all); supports `?status=pending|in_progress|completed&page=1&limit=20`
- `POST /tasks` — Create task
- `GET /tasks/:id` — Get single task
- `PUT /tasks/:id` — Update task
- `DELETE /tasks/:id` — Delete task

### Admin (JWT + admin role required)
- `GET /admin/users` — List all users

### Docs
- `GET /api/api-docs` — Swagger UI

## Role-Based Access

- **user**: Can only CRUD their own tasks
- **admin**: Can read/manage ALL tasks and view all users
- Roles enforced via `authenticate` + `requireAdmin` middleware

## TypeScript & Composite Projects

Every package extends `tsconfig.base.json`. Root `tsconfig.json` lists lib packages as project references.

- `pnpm run typecheck` — full typecheck
- `pnpm run build` — build all packages

## Packages

### `artifacts/api-server` (`@workspace/api-server`)

Express 5 API. JWT + bcrypt auth, Swagger docs, RBAC middleware.

- `pnpm --filter @workspace/api-server run dev` — run dev server
- `pnpm --filter @workspace/api-server run build` — production build

### `artifacts/task-manager` (`@workspace/task-manager`)

React + Vite frontend. Login, register, task dashboard, admin panel.

- `pnpm --filter @workspace/task-manager run dev` — run dev server

### `lib/db` (`@workspace/db`)

Drizzle ORM with PostgreSQL. Users + Tasks schema.

- `pnpm --filter @workspace/db run push` — push schema changes to DB

### `lib/api-spec` (`@workspace/api-spec`)

OpenAPI 3.1 spec + Orval codegen config.

- `pnpm --filter @workspace/api-spec run codegen` — regenerate client hooks + Zod schemas
