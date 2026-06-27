# marketplace-web

Build + deploy pipeline for **[marketplace.langstitch.com](https://marketplace.langstitch.com)**.

This repo contains no application source — it builds the LangStitch **marketplace
SPA** from the canvas (which lives in the private `LangStitch/langtailor` repo
under `_canvas/`) and deploys the static build to Hostinger. The SPA detects the
host at runtime and renders the standalone marketplace when served from
`marketplace.langstitch.com`.

## How it works

`.github/workflows/deploy.yml` on every push to `main` (or manual run):

1. Checks out the private `langtailor` repo (needs `CANVAS_REPO_TOKEN`).
2. Builds `_canvas` with `VITE_PLATFORM_API` pointing at the backend API.
3. FTPS-deploys `_canvas/dist/` to the marketplace docroot on Hostinger.

## Required configuration

Secrets:

| Secret | Purpose |
|--------|---------|
| `CANVAS_REPO_TOKEN` | PAT with read access to `LangStitch/langtailor` |
| `FTP_SERVER` / `FTP_USERNAME` / `FTP_PASSWORD` | Hostinger FTP |

Variables:

| Variable | Example |
|----------|---------|
| `PLATFORM_API_BASE` | `https://api.langstitch.com` |
| `FTP_SERVER_DIR` | the marketplace subdomain docroot (e.g. `public_html/`) |

> The backend API must be live at `PLATFORM_API_BASE` with
> `LANGSTITCH_AUTH_ENABLED=true` + MySQL, and its `LANGSTITCH_CORS_ORIGINS`
> must include `https://marketplace.langstitch.com`. See
> `LangStitch/langstitch-backend-service`.
