# Docker usage

This repo now ships Docker assets for both the FastAPI backend (`nomada`) and the Vite frontend (`frontend`) via `docker-compose.yml`.

## Prerequisites
- Docker and Docker Compose installed

- A populated `nomada/.env` with runtime secrets, e.g.:
  - `OPENAI_API_KEY=...` (required)
  - `DUFFEL_API_TOKEN=...` (required)
  - `HUGGINGFACE_API_KEY=...` (required)
  - `GOOGLE_MAPS_API_KEY=...` (required)
  - `HOTELBEDS_API_KEY=...` (required)
  - `HOTELBEDS_SECRET=...` (required)
  - `SMTP_HOST=...`, `SMTP_PORT=587`, `SMTP_USER=...`, `SMTP_PASS=...`, `SMTP_FROM=...` (optional, for booking emails)


## Start Docker
Ensure Docker Desktop/daemon is running. On Windows/Mac, open Docker Desktop; on Linux, start the Docker service (`sudo service docker start`). You can verify with:
```bash
docker info
```

## Build
```bash
docker-compose build
```
The frontend build embeds `VITE_API_URL` (default `http://backend:8000`); override at build time if needed:
```bash
docker-compose build --build-arg VITE_API_URL=http://localhost:8000 frontend
```

## Run
```bash
docker-compose up
```
- Backend: http://localhost:8000 (FastAPI, served by `uvicorn api_server:app`)
- Frontend: http://localhost:4173 (static build served by nginx)

The backend container mounts `./nomada/databases` to `/app/databases` to persist SQLite data locally.

> Note: When running the frontend in your browser, the API must be reachable from your host. The compose file sets `VITE_API_URL` to `http://localhost:8000` by default so browser requests hit the exposed backend port.

## Common tweaks
- To run detached: `docker-compose up -d`
- Rebuild after code changes: `docker-compose build frontend backend`
- Inspect logs: `docker-compose logs -f backend` (or `frontend`)
