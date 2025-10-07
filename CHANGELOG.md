# Changelog

All notable changes to this project will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- (placeholder) Add new entries here for the next version.

## [0.1.1] - 2025-09-22

### Added

- Responsive, mobile-first navbar with **full-width Bootstrap-style collapse** on small screens.
- `nav_links` **Jinja macro** to render desktop and mobile menus from a single source, with active-link state.
- `.wrap` utility updated to **alias Tailwind’s `container`** (breakpoints + padding) so header/main/footer align.

### Changed

- `base.html` now includes the navbar partial and consistent header/footer wrappers.

### Fixed

- Registered `now()` as a Jinja global (`{{ now().year }}`) to prevent `UndefinedError`.
- Dev CSS rebuilds: switched to **chokidar-cli** watcher and ignored temp files (`._*`, swap files) for stable HMR.
- Frontend watcher shell: set `SHELL=/bin/sh` in the Node image so chokidar can execute commands reliably.

## [0.1.0] - 2025-09-??

### Added

- Initial MVP with dev and prod profiles.
- Hot reload for FastAPI (backend) and Tailwind/Alpine (frontend).
- Self-contained production image with baked CSS/JS.
- Docker Compose setup with `dev`, `prod`, and `pull` profiles.
- Example templates (`base.html`, `index.html`, `404.html`).
- `.env.example` with internal/external port variables.
- Documentation in `README.md`.
