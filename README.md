# Brandon Holdings Group — Admin

Control panel for the
[Brandon Holdings Group static site](https://github.com/Re-EL123/Brandon-Holdings-Group-Website).
A single static `admin.html` that talks to the
[brandonholdingsgroup-api](https://github.com/Re-EL123/brandonholdingsgroup-api)
middleman — it never talks to Firebase directly and holds no secrets.

## Features

- **Email settings** — set who receives form submissions (`to_email`) and the
  sender address (`from_email`). Saved to Firebase Firestore via the API.
- **Contact details** — change the address, email, phone, WhatsApp and
  social links shown on the website, remotely. The site picks them up from
  `GET /api/settings` on every page load.
- **Stats** — total / per-page / per-day page views, recent page views, and
  logs of emails sent and form submissions.

## Setup

1. Point the UI at the API deployment. It uses the same origin by default,
   or pass the API base URL explicitly:

   - `?api=https://your-api.example.com` query parameter, or
   - `window.ADMIN_API_BASE = 'https://your-api.example.com';` global.

2. When prompted, enter the API's `ADMIN_TOKEN` (the same value configured on
   the API server). The token is kept in `sessionStorage` for that tab.

## Running locally

```bash
python3 -m http.server 8081
# open http://localhost:8081/admin.html?api=http://localhost:8080
```

or use any static host / Vercel. The admin repo has no runtime code of its
own — just this page.

## Deploying

Host the directory as a static Vercel/Netlify/GitHub Pages project, then open
the deployed `admin.html` with the API base appended:

```
https://<admin-host>/admin.html?api=https://<api-host>
```

The API's CORS is permissive so the page works from any origin. To avoid the
query string, deploy the admin page on the same domain as the API and it will
default to that origin. `scripts/deploy.sh` from the API repo can deploy all
three pieces at once.

## Security notes

- The admin token is the gate for all `/api/admin/*` calls. Protect it.
- CORS on the API is permissive so this page can live on any origin; keep the
  API deployment behind HTTPS and set a strong `ADMIN_TOKEN`.
