# NetBox

NetBox uses the official [netbox-docker](https://github.com/netbox-community/netbox-docker) project, cloned on the server (`git clone -b release ...`). I don't copy their files here, only my override.

Changes I make before the first start:

- `env/netbox.env`: real `SECRET_KEY`, `DB_PASSWORD`, `CSRF_TRUSTED_ORIGINS=https://netbox.home.example.com`
- `env/postgres.env`: same DB password
- `docker-compose.override.yml` (below) to publish the web port
