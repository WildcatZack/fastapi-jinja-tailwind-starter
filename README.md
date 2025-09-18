# FastAPI + Jinja2 + Tailwind v4 + Alpine + Docker (Starter)

This project is an **easy way to spin up a development environment** with FastAPI, Jinja2 templates, Tailwind v4, AlpineJS, and Docker.  
It can serve as a **guide to learn from** or a **full template to start projects with**.

---

## Environment

Copy and edit:

    cp .env.example .env

`.env` variables:

    UVICORN_INTERNAL=8000
    UVICORN_EXTERNAL=8000
    UVICORN_INTERNAL_DEV=8000
    UVICORN_EXTERNAL_DEV=18000
    # For pull profile tests (pick one and set):
    # IMAGE_REF=ghcr.io/<user>/<repo>:v0.1.0
    # IMAGE_REF=<dockerhub_user>/<repo>:v0.1.0

---

## Run — Dev (hot reload)

    docker compose --profile dev up --build

- App: http://localhost:${UVICORN_EXTERNAL_DEV}/
- Health: http://localhost:${UVICORN_EXTERNAL_DEV}/health

---

## Build & Run — Prod (self-contained image)

    docker compose --profile prod up --build -d

- App: http://localhost:${UVICORN_EXTERNAL}/
- Health: http://localhost:${UVICORN_EXTERNAL}/health

> The prod image includes Python + templates + built CSS/JS. No Node or bind mounts at runtime.

---

## Push → Pull (validate image portability)

1.  Build prod locally:

         docker compose --profile prod build fjts-backend-prod

2.  Tag & push to your registry (GHCR example):

         # Set IMAGE_REF in .env, e.g. ghcr.io/<user>/<repo>:v0.1.0
         echo <GHCR_PAT> | docker login ghcr.io -u <GH_USER> --password-stdin
         docker tag fastapi-jinja-tailwind-starter-fjts-backend-prod:latest ${IMAGE_REF}
         docker push ${IMAGE_REF}

3.  Run the **pull** profile (no build step; pulls the image and runs it):

         docker compose --profile pull up -d fjts-backend-pull

---

## Troubleshooting

- **Dev CSS doesn’t update** → Save again, and if needed do a browser **Hard Reload**.
- **Healthcheck 405** → `curl -sI` uses `HEAD`. Our `/health` is `GET`-only; use:  
   curl -s http://localhost:${UVICORN_EXTERNAL}/health

---

## Future Improvements

- Mobile-first responsive layout in base template.
- Multiple `base.html` templates to demonstrate flexible layouts.
- Dark/light theme toggle (Alpine-driven).
- More sample Jinja pages (e.g. `/about`, `/contact`).
- Production hardening (non-root user, healthcheck baked in).
- Optional devcontainer / CI/CD setup.
- Swapable CSS frameworks (showing Tailwind vs. Bootstrap starter).

---

## License

MIT
