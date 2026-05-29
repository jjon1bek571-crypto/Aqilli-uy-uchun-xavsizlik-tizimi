# EarlyAlert — Safety, before it's too late

An intelligent early-warning layer for **fire** and **water damage** in homes and
buildings. EarlyAlert monitors sensors, raises alerts, runs AI safety audits and
connects people to emergency response — all in one fast, installable web app.

> **v3.0** — full rewrite: brand-new design system, real backend (serverless AI
> proxy + Supabase auth), production build pipeline, PWA support, and a
> trilingual UI (Oʻzbek / Русский / English).

---

## Nima qilingan (v3 yangilanishi)

Bu versiyada loyiha demo'dan to'liq mahsulot darajasiga ko'tarildi:

- **Xavfsizlik** — Gemini API kaliti endi brauzerda emas, server tomonida (`/api`)
  saqlanadi. Ilgari kalit kod ichida ochiq edi.
- **Backend** — Vercel serverless funksiyalari (AI proksi) + Supabase (autentifikatsiya
  va maʼlumotlar bazasi). Kalitlarsiz ham ilova **demo rejimda** to'liq ishlaydi.
- **Yangi dizayn** — yorug', zamonaviy, izchil dizayn tizimi; yorug'/qorong'u rejim.
- **Optimal build** — Tailwind CDN o'rniga to'liq build, kod bo'linishi, PWA, oflayn.
- **Routing, holat boshqaruvi, xatoliklarni ushlash** va trilingual interfeys.

---

## Features

- **Live dashboard** — protection score, sensor status, 24-hour trend charts.
- **Fire & Water monitoring** — smoke, temperature, humidity and flow, with a
  remote main-valve control.
- **AI Risk Audit** — upload a photo of a room and get a structured hazard report
  (powered by Google Gemini, proxied server-side).
- **Live Expert** — real-time voice safety assistance over the Gemini Live API
  using short-lived ephemeral tokens (the key never reaches the browser).
- **Alerts & response map** — event history plus nearest emergency-station ETA.
- **Automation, Community, Academy, Business** — safety scenarios, trusted
  contacts, lessons and B2B partnership modules.
- **PWA** — installable, works offline, light/dark theme, 3 languages.

## Tech stack

| Layer     | Technology                                            |
| --------- | ----------------------------------------------------- |
| Frontend  | React 19, TypeScript, Vite 6, Tailwind CSS v4         |
| Routing   | React Router v7                                       |
| State     | Zustand (persisted)                                   |
| Charts    | Recharts                                              |
| Backend   | Vercel Serverless Functions (`/api`)                  |
| AI        | Google Gemini (`@google/genai`) — server-side only    |
| Auth + DB | Supabase (PostgreSQL + Row Level Security)            |

---

## Quick start

```bash
npm install
npm run dev
```

Open <http://localhost:3000>. With **no configuration at all** the app runs in
**demo mode**: a guest account, seeded sensors/alerts, and canned AI responses.

## Configuration

Copy `.env.example` to `.env.local` and fill in what you need. Every value is
optional — missing values simply keep that feature in demo mode.

| Variable                 | Where    | Purpose                                  |
| ------------------------ | -------- | ---------------------------------------- |
| `GEMINI_API_KEY`         | server   | Enables real AI (risk audit, live voice) |
| `GEMINI_MODEL_TEXT`      | server   | Override the analysis model              |
| `GEMINI_MODEL_IMAGE`     | server   | Override the image-simulation model      |
| `GEMINI_MODEL_LIVE`      | server   | Override the live-voice model            |
| `VITE_SUPABASE_URL`      | client   | Enables real auth + cloud sync           |
| `VITE_SUPABASE_ANON_KEY` | client   | Supabase anon key (public by design)     |

### AI setup (Gemini)

1. Get a key at <https://aistudio.google.com/apikey>.
2. Set `GEMINI_API_KEY` in `.env.local` (local) or in Vercel project settings.
3. The key is read **only** by the functions in `/api` — it is never bundled
   into the browser.

### Supabase setup (optional — for real accounts)

1. Create a free project at <https://supabase.com>.
2. In the SQL editor, run [`supabase/schema.sql`](./supabase/schema.sql). It
   creates the tables, Row Level Security policies and a trigger that provisions
   a profile for every new user.
3. Copy the project URL and `anon` key into `VITE_SUPABASE_URL` /
   `VITE_SUPABASE_ANON_KEY`.

## Deploy (Vercel)

1. Push this repository to GitHub.
2. Import it at <https://vercel.com/new> — the framework preset (`vite`) and the
   `/api` functions are picked up automatically from `vercel.json`.
3. Add the environment variables in **Project → Settings → Environment Variables**.
4. Deploy. The SPA rewrite and security headers are already configured.

## Scripts

| Command            | Description                          |
| ------------------ | ------------------------------------ |
| `npm run dev`      | Start the dev server (port 3000)     |
| `npm run build`    | Type-check and build for production  |
| `npm run preview`  | Preview the production build         |
| `npm run typecheck`| Run the TypeScript compiler only     |

## Project structure

```
early-alert/
├── api/                 Serverless functions (AI proxy — keeps keys server-side)
│   ├── analyze.ts        POST /api/analyze       — home risk audit
│   ├── edit-image.ts     POST /api/edit-image    — visual simulation
│   ├── live-token.ts     POST /api/live-token    — ephemeral live-voice token
│   └── health.ts         GET  /api/health        — status probe
├── supabase/
│   └── schema.sql        Database schema + RLS policies
├── src/
│   ├── components/       ui/ (design system) · layout/ · feature/
│   ├── pages/            One file per route
│   ├── store/            Zustand stores (auth, ui, data)
│   ├── lib/              supabase, api client, utils, demo data, nav
│   ├── hooks/            i18n hook
│   ├── i18n/             uz / ru / en translations
│   └── types/            Shared domain types
└── public/               PWA manifest, service worker, icon
```

## Security notes

- The Gemini API key lives only in serverless functions — it is **never** exposed
  to the client bundle.
- Supabase access is guarded by Row Level Security: every user can read and write
  only their own rows.
- Security headers (`X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`,
  `Permissions-Policy`) are set in `vercel.json`.

## Roadmap ideas

- Real-time sensor ingestion (MQTT / webhook → Supabase Realtime).
- Push notifications for critical alerts.
- Automated test suite (Vitest + Testing Library).

---

Built with care for safety. EarlyAlert — detect, decide, respond.
