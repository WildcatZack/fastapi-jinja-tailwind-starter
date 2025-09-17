FROM node:20-alpine

WORKDIR /workspace

# Ensure chokidar-cli has a shell to invoke
ENV SHELL=/bin/sh

# Keep node_modules inside the image
COPY frontend/package.json frontend/package-lock.json* ./frontend/
RUN cd frontend && npm install

# Add default sources (dev mounts will override at runtime)
COPY frontend/src ./frontend/src
COPY backend/app/templates ./backend/app/templates

WORKDIR /workspace/frontend
CMD ["npm","run","dev"]
