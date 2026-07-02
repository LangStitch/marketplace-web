# marketplace-web

Build + deploy pipeline for **[marketplace.langstitch.com](https://marketplace.langstitch.com)**.

This repo contains no application source — it builds the LangStitch **marketplace
SPA** from the canvas (which lives in the private `LangStitch/langtailor` repo
under `_canvas/`) and deploys the static build to Hostinger. The SPA detects the
host at runtime and renders the standalone marketplace when served from
`marketplace.langstitch.com`.

## How it works

`.github/workflows/deploy-hostinger.yml` on every push to `main` (or manual run):

1. Checks out the private `langtailor` repo (needs `CANVAS_REPO_TOKEN`).
2. Builds `_canvas` with `VITE_PLATFORM_API` pointing at the backend API.
3. FTPS-deploys `_canvas/dist/` to the marketplace docroot on Hostinger.

GitHub Pages is not used for this SPA — Hostinger serves `marketplace.langstitch.com`.

## Required configuration

Secrets:

> The marketplace SPA (`marketplace-web`) calls this API. MySQL credentials
> live in `langstitch-backend-service` secrets — not in `marketplace-web`.

| Secret | Purpose |
|--------|---------|
| `CANVAS_REPO_TOKEN` | PAT with read access to `LangStitch/langtailor` |
| `FTP_SERVER` | `217.21.84.75` (no `ftp://` prefix) |
| `FTP_USERNAME` | `u743467360.marketplacedeveloper` |
| `FTP_PASSWORD` | FTP password for that account |

Variables:

| Variable | Default |
|----------|---------|
| `PLATFORM_API_BASE` | `https://api.langstitch.com` |

> The backend API must be live at `PLATFORM_API_BASE` with
> `LANGSTITCH_AUTH_ENABLED=true` + MySQL, and its `LANGSTITCH_CORS_ORIGINS`
> must include `https://marketplace.langstitch.com`. See
> `LangStitch/langstitch-backend-service`.
