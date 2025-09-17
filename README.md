# FastAPI + Jinja2 + Tailwind v4 + Alpine + Docker (Starter)

Self-contained starter with Docker Compose profiles for **dev** (hot reload + watchers) and **prod** (single image with built assets).  
No databases, no CI — just the essentials.

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

**Notes**

- CSS/JS build live into `backend/app/static/{css,js}`.
- If CSS changes don’t appear, do a browser **Empty Cache and Hard Reload** (Chrome).

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

## License

MIT
