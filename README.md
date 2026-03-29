<div align="center">
### Hospital DBMS Operations Console
#### Designed & crafted by **Syed Daniyal Haider Naqvi**

![Stack](https://img.shields.io/badge/Stack-Vite%20%7C%20React%20%7C%20Express%20%7C%20MongoDB-0a9396)
![Status](https://img.shields.io/badge/Status-Active-success)
![License](https://img.shields.io/badge/License-MIT-informational)

Real-time hospital intelligence on top of MongoDB Atlas: aggregation pipelines, dashboards, collections browser, vulnerability sandbox, and more—all with a slick shadcn UI.

</div>

## 🧭 Quick Links

- [Highlights](#-highlights)
- [Tech Stack](#-tech-stack)
- [Setup](#-getting-started)
- [Aggregation Builder](#-aggregation-builder-deep-dive)
- [Vulnerability Lab](#-vulnerability-lab-explained)
- [Troubleshooting](#-troubleshooting)

## ✨ Highlights

- **Atlas-native analytics** – each page hits the live cluster via curated REST + WebSocket pipes.
- **Aggregation Builder 2.0** – stage builder, raw mode, index manager, execution stats, preset pipelines.
- **Collections cockpit** – search, paginate, edit, and delete with toast-backed feedback loops.
- **Dashboards & Admin** – KPI cards, trend charts, audit logs, and staff metrics in one place.
- **Vulnerability Lab** – safe playground to demonstrate NoSQL injection vectors vs. mitigations.
- **Resilient auth** – Supabase-powered when configured, otherwise auto-falls back to mock auth (with creator-only admin access `naqvidaniyal598@gmail.com` / `dani007`).

## 🧱 Tech Stack

| Layer | Love poured in |
| --- | --- |
| Frontend | React 18 · Vite · TypeScript · Tailwind · shadcn/ui · Lucide |
| Backend | Express · MongoDB Node driver · ws · Zod · change streams |
| Auth | Supabase (optional) + secure local-storage fallback |

## 🗂 Architecture Snapshot

```
client (Vite/React)
 ├─ contexts/AuthContext.tsx (Supabase + mock fallback)
 ├─ hooks/useMongoDb.ts (all REST calls & aggregation helpers)
 ├─ hooks/useRealtime.ts (change streams via WS)
 └─ pages/* (Dashboard, Collections, Aggregations, Vulnerability Lab, ...)

server (Express + ws)
 ├─ routes/collections.ts (CRUD, aggregation, indexes, stats)
 ├─ routes/dashboard.ts / admin.ts
 ├─ mongoClient.ts (primary + stats-only clients)
 └─ index.ts (REST + WebSocket bootstrap)
```

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ (20+ recommended)
- npm 9+
- MongoDB Atlas URI with read/write + change-stream privileges
- Optional Supabase project (URL + anon key)

### Installation

```sh
git clone https://github.com/DaniyalSE/Hospital_DBMS.git

npm install
cp .env.example .env
# edit .env with Atlas + Supabase secrets (or leave Supabase empty for mock auth)

npm run dev
```

`npm run dev` spins up Vite (port 8080, auto-fallback) and Express/WebSocket (`SERVER_PORT`, default 4000) via `concurrently`. Use `npm run dev:client` or `npm run dev:server` to isolate either side.

## 🔐 Environment Variables

| Key | Required | Notes |
| --- | --- | --- |
| `MONGODB_URI` | ✅ | Atlas URI with change-stream access. |
| `MONGODB_DB` | ✅ | Target database (default `HospitalDB`). |
| `SERVER_PORT` | ✅ | Express/WebSocket port (default 4000). |
| `VITE_SUPABASE_URL` | ⚙️ | Omit to rely on mock auth. |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | ⚙️ | Supabase anon/publishable key. |
| `VITE_API_BASE_URL` | optional | Override API origin in prod deployments. |
| `VITE_WS_URL` | optional | Override WebSocket origin in prod. |

Mock auth ensures the demo remains usable offline. Only the creator’s credentials (`naqvidaniyal598@gmail.com` / `dani007`) unlock admin mode; every other signup/login becomes guest.

## 🧪 Aggregation Builder Deep Dive

| Feature | Why it matters |
| --- | --- |
| Stage Builder ↔ Raw JSON | Swap between low-code editing and full pipeline control instantly. |
| Index Manager | View all indexes, create `{ keys, options }`, drop safely with confirmations and toasts. |
| Execution Stats | Auto-runs `explain("executionStats")`, displaying exec time, docs/keys examined, winning plan, and raw explain JSON. |
| Preset Pipelines | Jump-start analysis with curated scenarios (gender counts, department loads, etc.). |
| Safety Rails | JSON validation, result truncation warnings, clipboard helpers, admin-only save button. |

## 🛡️ Vulnerability Lab Explained

- Toggle **Secure** vs **Vulnerable** to simulate parameterized queries vs raw JSON injection.
- Paste payloads like `{ "$gt": "" }` or `{ "$where": "1==1" }` to watch how attacks would slip through.
- Alerts + toasts explain whether the input was blocked or executed.
- 100% frontend sandbox—no real collections are harmed. Purely for defensive education.

## 📜 npm Scripts

| Command | What it does |
| --- | --- |
| `npm run dev` | Vite + Express in tandem. |
| `npm run dev:client` | Frontend only. |
| `npm run dev:server` | API + WebSocket only. |
| `npm run build` | Build client + compile server to `dist/`. |
| `npm run build:server` | Compile server alone. |
| `npm run preview` | Serve the production client bundle. |
| `npm start` | Run the compiled Express server from `dist/server/index.js`. |

## 🛠 Troubleshooting

- **API refuses to start** – verify `.env` and ensure your Atlas IP allow list includes the dev machine.
- **Frontend says “Connected” but cards are empty** – check API logs; usually a missing collection or invalid pipeline.
- **Explain errors** – resolved via the dual Mongo clients (stats client without writeConcern). Restart the server if you tweak env vars.
- **“Large result set” toast** – friendly reminder that only the first 500 docs are rendered. Add `$limit`, `$project`, or `$count` for focused payloads.

## 📦 Deployment Flow

1. `npm run build`
2. Copy `dist/` to your server/hosting platform.
3. `npm start` (or `pm2 start dist/server/index.js`) with the same `.env` values.
4. Expose the Express port through your reverse proxy and configure `VITE_API_BASE_URL` / `VITE_WS_URL` when serving the client separately.

## 👤 Author

Crafted with ❤️ by **Syed Daniyal Haider Naqvi**. Feel free to fork, extend, and share your own hospital analytics ideas—PRs and feedback are always welcome!

