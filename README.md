# FastAPI + Jinja2 + Tailwind v4 + Alpine + Docker (Starter)

Self-contained starter with Docker Compose profiles for **dev** (hot reload + watchers) and **prod** (single image with built assets).  
No databases, no CI — just the essentials.

---

## Repository Layout

/  
&nbsp;&nbsp;backend/  
&nbsp;&nbsp;&nbsp;&nbsp;app/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;templates/ # Jinja pages (scanned by Tailwind)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;static/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;css/app.css # built by Tailwind CLI  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;js/app.js # built by esbuild  
&nbsp;&nbsp;&nbsp;&nbsp;main.py  
&nbsp;&nbsp;&nbsp;&nbsp;routers/  
&nbsp;&nbsp;frontend/  
&nbsp;&nbsp;&nbsp;&nbsp;src/  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;input.css # Tailwind entry (v4, config-less)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;alpine.entry.js # Alpine init (bundled)  
&nbsp;&nbsp;&nbsp;&nbsp;Dockerfile # dev watcher image  
&nbsp;&nbsp;&nbsp;&nbsp;package.json  
&nbsp;&nbsp;backend/Dockerfile.dev # dev backend (reload)  
&nbsp;&nbsp;backend/Dockerfile.prod # prod backend (multi-stage; assets baked in)  
&nbsp;&nbsp;compose.yml  
&nbsp;&nbsp;.env.example  
&nbsp;&nbsp;.gitignore  
&nbsp;&nbsp;LICENSE  
&nbsp;&nbsp;README.md

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
