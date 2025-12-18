# bb-trudesk

This repository is used to track the setup and deployment of a self-hosted TruDesk instance for BillingBase support.

## Purpose

- Host TruDesk (agent dashboard + API) as a separate service (e.g. on Render)
- Allow BillingBase backend services to create and manage support tickets via TruDesk APIs

## Planned Deployment (high level)

TruDesk is typically deployed with:

- TruDesk application container
- MongoDB (recommended: MongoDB Atlas)
- Elasticsearch (for search)

> Note: Render does not run `docker-compose` directly as a single unit. We will deploy TruDesk and Elasticsearch as separate Render services and use an external MongoDB instance.

## Next steps

- Add a reference `docker-compose.yml` (for local development/testing)
- Document Render service configuration (env vars, ports, persistence)
- Capture TruDesk admin setup notes (API token, group/type/priority IDs)
