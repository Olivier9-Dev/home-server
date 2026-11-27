# Stacks

Every service has its own folder with a `compose.yaml`. On the server they live in `/opt/stacks/<name>/`, with the app's data right next to the compose file. One folder = one thing to back up, move or delete.

| VM | Stacks |
|---|---|
| `docker-core` | `npm`, `uptime-kuma`, `ntfy`, `dozzle`, `diun`, `netalertx`, `homepage`, `it-tools`, `searxng` |
| `docker-apps` | `jellyfin`, `dozzle-agent`, `diun` |

Starting a stack:

```bash
docker network create proxy          # once per VM, shared with Nginx Proxy Manager
cp .env.example .env && chmod 600 .env
nano .env                            # real values, never committed
docker compose config                # check the file
docker compose up -d
```

Rules I follow:

- Anything with a web UI joins the `proxy` network and is reached through Nginx Proxy Manager over HTTPS.
- Tools that only need to read Docker get the socket **read-only** (`:ro`).
- Secrets (tokens, domain) live in `.env`, which is git-ignored. Only `.env.example` is committed.
- Updates: Diun tells me, I read the release notes, then `docker compose pull && docker compose up -d`, one stack at a time.
