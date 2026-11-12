# Stacks

Every service has its own folder with a `compose.yaml`. On the server they live in `/opt/stacks/<name>/`, with the app's data right next to the compose file. One folder = one thing to back up, move or delete.

Before starting a stack:

```bash
docker network create proxy          # once per VM, shared with Nginx Proxy Manager
cp .env.example .env && chmod 600 .env
nano .env                            # real values, never committed
docker compose config                # check the file
docker compose up -d
```

Rules I follow:

- Pin database major versions (`postgres:17`). Never upgrade a DB major in place.
- One database container per app, on a private `internal` network.
- Anything that only needs a web UI goes behind Nginx Proxy Manager on the `proxy` network.
- Updates: Diun tells me, I read the release notes, then `docker compose pull && docker compose up -d`.
