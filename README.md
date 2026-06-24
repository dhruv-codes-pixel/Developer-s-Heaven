# SiteStock

A single-file React + Supabase app for tracking material deliveries and costs across construction job sites / projects.

## How it's built

- Plain `index.html` — React (via CDN, no build step), Supabase JS client (via CDN), all CSS inline.
- No `npm`, no bundler, no `package.json`. The whole app is one HTML file.
- Data persistence and realtime sync are handled by Supabase (`projects` and `material_logs` tables).
- Login is a local username/password check (not Supabase Auth) — see the `ADMIN CREDENTIALS` section near the top of the `<script>` in `index.html` for current Viewer/Owner usernames and passwords.

## Deploying

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

### 2. Deploy on Vercel

1. Go to [vercel.com/new](https://vercel.com/new) and import the GitHub repo.
2. Framework preset: leave as **Other** (or it will auto-detect). No build command, no install command, no output directory override needed — `index.html` is served as-is from the repo root.
3. Click **Deploy**.

That's it — no environment variables are needed at build time, since the Supabase URL and key are already embedded directly in `index.html`.

## ⚠️ Before making the repo public / sharing the deployed URL

This app has no real backend auth — anyone who visits the deployed site can open **View Page Source** and read:

- The Supabase URL and anon/publishable key.
- Both the Viewer and Owner usernames/passwords, in plain text.

Right now, Row Level Security (RLS) is **disabled** on `projects` and `material_logs`, which means the Supabase anon key alone is enough for anyone to read or write **any** row in those two tables — not just their own.

If this is just for personal/team use behind an unlisted URL, that's a reasonable tradeoff. If you want it locked down further, options include:

- **Keep the GitHub repo private** (doesn't protect the live Vercel URL itself, but stops people from finding the source via GitHub search).
- **Vercel Password Protection** (Pro/Team plans) — gates the entire deployed site behind a single shared password before anyone reaches the app.
- **Re-enable RLS with anon-scoped policies** instead of disabling it outright, so reads/writes are still allowed without a real Supabase Auth session, but scoped more deliberately.
