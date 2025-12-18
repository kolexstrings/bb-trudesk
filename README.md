# bb-trudesk

This repository is used to track the setup and deployment of a self-hosted TruDesk instance for BillingBase support.

## Why you don't see the TruDesk "project files" here

This repo does not contain TruDesk source code.

We run TruDesk using a prebuilt Docker image (`polonel/trudesk:1`). When you run `docker compose up`, Docker pulls that image from a remote container registry and stores it locally in Docker Desktop.

Useful commands:

- `docker images | grep trudesk`
- `docker compose pull`

## Purpose

- Host TruDesk (agent dashboard + API) as a separate service (e.g. on Render)
- Allow BillingBase backend services to create and manage support tickets via TruDesk APIs

## Planned Deployment (high level)

TruDesk is typically deployed with:

- TruDesk application container
- MongoDB (recommended: MongoDB Atlas)
- Elasticsearch (for search)

> Note: Render does not run `docker-compose` directly as a single unit. We will deploy TruDesk and Elasticsearch as separate Render services and use an external MongoDB instance.

## Local development (repeatable runbook)

Prerequisites:

- Docker Desktop installed and running

Start the local stack:

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
docker ps --format 'table {{.Names}}\t{{.Status}}\t{{.Ports}}'
```

View logs:

```bash
docker compose logs -f --tail=200 trudesk
docker compose logs -f --tail=200 mongo
docker compose logs -f --tail=200 elasticsearch
```

Open TruDesk:

- http://localhost:8118

Stop services (keep data):

```bash
docker compose down
```

Stop services and delete all local data (factory reset):

```bash
docker compose down -v
```

Upgrade images (pull newest tags referenced in compose) and restart:

```bash
docker compose pull
docker compose up -d
```

### Data persistence

This compose file uses named volumes, so data persists across restarts:

- `trudesk_uploads` (attachments)
- `trudesk_backups`
- `mongo`, `mongo_data`
- `elasticsearch`

## Next steps

- Add a reference `docker-compose.yml` (for local development/testing)
- Document Render service configuration (env vars, ports, persistence)
- Capture TruDesk admin setup notes (API token, group/type/priority IDs)

## Production notes (DigitalOcean later)

- Do not expose MongoDB / Elasticsearch ports publicly.
- Put TruDesk behind HTTPS (Caddy/Nginx/Traefik) and use a domain like `support.billingbase.io`.
- Back up MongoDB + uploads volume.
