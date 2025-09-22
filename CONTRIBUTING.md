# Contributing Guide

Thanks for contributing! This repo is a **template + guide** for getting a FastAPI + Jinja + Tailwind v4 + Alpine dev environment running quickly. We keep changes small, tagged, and documented.

---

## Workflow Overview

1. Create a feature branch off `master`

   - Examples:
     - `feat/responsive-navbar`
     - `feat/dark-mode-toggle`
     - `docs/update-readme`

2. Commit style (light conventional)

   - `feat: add theme toggle macro`
   - `fix: correct Tailwind sources for prod build`
   - `docs: update README (future improvements)`

3. Update the changelog **before** opening a PR

   - Add entries under **[Unreleased]** in `CHANGELOG.md`:
     - `### Added` / `### Changed` / `### Fixed`

4. Open a Pull Request into `master`

   - Keep PRs small and focused.
   - Use a descriptive title that mirrors the changelog line.
   - Mark as draft if still iterating.

5. Keep your branch up to date via **rebase** (recommended)

   - Before/while your PR is open, rebase on `origin/master` to pull in the latest changes.
   - Force-push the feature branch with **`--force-with-lease`** (safe for _your_ branch).

6. Merge → Tag
   - After merge, move `[Unreleased]` entries into a new version section (e.g., `0.1.1`) with today’s date.
   - Create an annotated tag `v0.x.y` and push it.
   - Optionally draft a GitHub Release and paste the changelog section.

---

## Setup (Dev & Prod quick refs)

Dev (hot reload):

    docker compose --profile dev up --build

Prod (self-contained image, no watchers):

    docker compose --profile prod up --build -d

Pull a previously pushed prod image (validation only):

    docker compose --profile pull up -d fjts-backend-pull

Environment variables live in `.env` (see `.env.example`), with separate internal/external ports for dev and prod.

---

## Rebase vs Merge

We prefer **rebase** for feature branches to keep a clean, linear history.

Rebase example (on your feature branch):

    git fetch origin
    git rebase origin/master
    # resolve conflicts if prompted:
    git add <file>
    git rebase --continue
    git push --force-with-lease

If you’d rather avoid rewriting history, merging `origin/master` into your feature branch is acceptable (it just adds merge commits).

---

## PR Checklist

- Builds in **dev** and **prod** profiles.
- For dev: CSS/JS rebuild on save (Tailwind + esbuild running).
- For prod: CSS/JS baked into the image (no Node at runtime).
- Updated **[Unreleased]** in `CHANGELOG.md`.
- README examples still accurate.
- Keeps to the repo layout and avoids committing secrets / `.env`.

---

## Versioning & Tags

- Maintain `CHANGELOG.md` using Keep a Changelog style.
- After merging to `master`, cut a tag:

  git tag -a vX.Y.Z -m "vX.Y.Z - <summary>"
  git push origin vX.Y.Z

We don’t publish template Docker images; tags are for the Git repo only.

---

## Code Style & Structure

- **Templates**

  - Use Jinja templating with `{% block %}`, `{% include %}`, and **macros** for reusable components.
  - Shared UI goes under `templates/partials/` and `templates/components/`.

- **Frontend**

  - Tailwind v4 (config-less) with sources defined in `frontend/src/input.css`.
  - AlpineJS for simple interactivity.
  - Dev uses chokidar + Tailwind CLI and esbuild watchers.
  - Prod builds assets via the multi-stage Dockerfile (no Node at runtime).

- **Backend**
  - FastAPI routes split under `backend/app/routers/`.
  - Static assets are served from `backend/app/static/{css,js}`.

---

## Questions

Open a discussion or a draft PR if you want early feedback. Small iterations win.
