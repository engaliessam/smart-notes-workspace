# 🗒️ Smart Notes Workspace

[![Project Status](https://img.shields.io/badge/status-active-brightgreen)](https://github.com)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react)](https://react.dev)
[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?logo=node.js)](https://nodejs.org)
[![MongoDB](https://img.shields.io/badge/MongoDB-7-47A248?logo=mongodb)](https://mongodb.com)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

A premium, full-stack note-taking application — final project for the **ITI Full Stack Developer** track. Features a glassmorphism UI, JWT auth, Markdown support, dark/light themes, and a fully documented REST API.

---

## 📽️ Demo

<video src="https://github.com/engaliessam/smart-notes-workspace/blob/db9c506f65656e65cf7535843767d70dd01e5c27/Project%20Demo.mp4"></video>

---

## 📁 Repository Structure

This workspace lives at the root level alongside both project folders.

```
smart-notes-workspace/          ← you are here (this README lives here)
├── smart-notes-api/            # Node.js + Express 5 REST API
│   ├── src/
│   ├── .env.example
│   ├── .gitignore
│   ├── package.json
│   └── postman_collection.json
├── smart-notes-web/            # React 19 + Vite SPA
│   ├── src/
│   ├── dist/
│   ├── index.html
│   ├── vite.config.js
│   ├── .env.example
│   ├── .gitignore
│   └── package.json
├── ITI_FullStack_Project_Requirements.pdf
├── Project Demo.mp4
└── README.md
```

> **Note on profile image uploads:** the API saves uploaded avatars to a local `uploads/` folder inside `smart-notes-api/`. This folder is gitignored and created automatically on first upload — no manual setup needed.

---

## ✨ Features

### 🖥️ Frontend (`smart-notes-web`)
- **Modern stack** — React 19 (Vite), React Router, Tailwind CSS with glassmorphism aesthetic
- **State management** — Redux Toolkit (auth & theme), TanStack Query (server state with optimistic updates)
- **Forms & validation** — React Hook Form + Zod schemas, fully typed
- **Notes** — Create, read, edit, delete, and pin; Markdown editor with live preview
- **Search & filters** — Debounced search (500 ms), filter by category & status, sort by date or A–Z, server-side pagination
- **Theme** — Dark / light mode, persisted across sessions
- **Profile** — Upload custom profile pictures
- **Tests** — Unit tests for custom hooks with Vitest

### ⚙️ Backend (`smart-notes-api`)
- **Layered architecture** — `routes → middleware → controller → model`
- **Auth** — JWT authentication, Bcrypt password hashing, protected middleware
- **Validation** — Strict server-side Zod validation before every controller
- **API docs** — Swagger UI at `/api-docs`
- **Notes** — Full CRUD, scoped per user (`userId`); supports search, category/status filter, sort, and pagination
- **Profile** — Multer-powered image uploads (saved to `smart-notes-api/uploads/`)

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 19, Vite, React Router, Redux Toolkit, TanStack Query, React Hook Form, Zod, Axios, Tailwind CSS, `@tailwindcss/typography`, react-markdown, Lucide icons |
| **Backend** | Node.js 18+, Express 5, MongoDB, Mongoose, JWT (`jsonwebtoken`), `bcryptjs`, Zod, Multer, Swagger (`swagger-jsdoc` + `swagger-ui-express`) |

---

## 🚀 Setup & Installation

### Prerequisites

- **Node.js 18+**
- **MongoDB** running locally, or a MongoDB Atlas connection string

---

### 1 — Clone the repository

```bash
git clone https://github.com/your-username/smart-notes-workspace.git
cd smart-notes-workspace
```

---

### 2 — Start the Backend

```bash
cd smart-notes-api
npm install
cp .env.example .env    # fill in the values shown below
npm run dev             # → http://localhost:5000
```

**`smart-notes-api/.env`**

| Key | Description | Example |
|---|---|---|
| `PORT` | Port the server listens on | `5000` |
| `MONGODB_URI` | MongoDB connection string (include db name) | `mongodb://localhost:27017/smart_notes` |
| `JWT_SECRET` | Secret used to sign JWTs | a long random string |
| `JWT_EXPIRES_IN` | Token lifetime | `7d` |

> **Swagger docs** will be available at `http://localhost:5000/api-docs` once the server is running.

**Scripts**

| Command | Description |
|---|---|
| `npm run dev` | Start with nodemon (development) |
| `npm start` | Start with Node (production) |

---

### 3 — Start the Frontend

Open a **new terminal tab**, then:

```bash
cd smart-notes-web
npm install
cp .env.example .env    # set VITE_API_URL to match your backend port
npm run dev             # → http://localhost:5173
```

**`smart-notes-web/.env`**

| Key | Description | Example |
|---|---|---|
| `VITE_API_URL` | Base URL of the backend API | `http://localhost:5000` |

> Vite only exposes variables prefixed with `VITE_` to the browser.

**Scripts**

| Command | Description |
|---|---|
| `npm run dev` | Start the dev server |
| `npm run build` | Production build into `dist/` |
| `npm run preview` | Preview the production build |
| `npm test` | Run Vitest unit tests |

---

## 🌐 API Reference

All `/notes` routes and `GET /auth/me` / `PATCH /auth/me` require an `Authorization: Bearer <token>` header.

### Auth

| Method | Route | Description | Auth? |
|---|---|---|---|
| `POST` | `/auth/register` | Create an account → `{ user, token }` | ❌ |
| `POST` | `/auth/login` | Log in → `{ user, token }` | ❌ |
| `GET` | `/auth/me` | Get current user | ✅ |
| `PATCH` | `/auth/me` | Update name / email / avatarUrl | ✅ |

### Notes

| Method | Route | Description | Auth? |
|---|---|---|---|
| `GET` | `/notes` | List notes (see query params below) | ✅ |
| `GET` | `/notes/:id` | Get a single note | ✅ |
| `POST` | `/notes` | Create a note | ✅ |
| `PATCH` | `/notes/:id` | Update a note | ✅ |
| `DELETE` | `/notes/:id` | Delete a note | ✅ |

**`GET /notes` query parameters**

| Param | Values | Description |
|---|---|---|
| `search` | any string | Full-text search on title / content |
| `category` | `Work` \| `Personal` \| `Ideas` \| `Learning` | Filter by category |
| `status` | `draft` \| `published` \| `archived` | Filter by status |
| `sort` | `updated` \| `created` \| `az` | Sort order |
| `page` | integer | Page number (default `1`) |
| `limit` | integer or `0` | Results per page (`0` = all) |

Response shape: `{ data, page, totalPages, total }`

> The full Postman collection is at `smart-notes-api/postman_collection.json`. Import it into Postman and set a `baseUrl` variable to `http://localhost:5000`.

---

## 🗄️ Data Models

### User
| Field | Type | Notes |
|---|---|---|
| `name` | String | Required |
| `email` | String | Unique |
| `password` | String | bcrypt hash — never returned in responses |
| `avatarUrl` | String | Path to uploaded image |
| `createdAt` / `updatedAt` | Date | Auto-managed |

### Note
| Field | Type | Notes |
|---|---|---|
| `title` | String | Required |
| `content` | String | Markdown |
| `category` | String | Work / Personal / Ideas / Learning |
| `status` | String | draft / published / archived |
| `isPinned` | Boolean | |
| `tags` | String[] | Comma-separated on input |
| `userId` | ObjectId | Scopes note to its owner |
| `createdAt` / `updatedAt` | Date | Auto-managed |

---

## 🏗️ Frontend Source Structure

```
smart-notes-web/src/
├── api/          # Axios instance + endpoint functions
├── app/          # Redux store
├── features/     # auth · notes · theme · ui (slices, hooks, components)
├── pages/        # One component per route
├── routes/       # ProtectedRoute wrapper
├── components/   # Shared UI — Layout, Toast, ConfirmModal
├── hooks/        # useDebouncedValue
└── styles/       # theme.css (design tokens + responsive rules)
```

---

## ✅ Bonus Features

- **Optimistic updates** — UI reacts instantly before backend confirmation (delete / edit on Dashboard)
- **Debounced search** — waits 500 ms before querying the server
- **Profile image upload** — multipart/form-data via Multer, stored in `smart-notes-api/uploads/`
- **Markdown rendering** — `@tailwindcss/typography` prose styles
- **Unit tests** — custom hooks tested with Vitest
- **Swagger docs** — full interactive endpoint UI at `http://localhost:5000/api-docs`

---

## 📄 License

[MIT](LICENSE)
