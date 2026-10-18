# Kortix is open source: deploy an alternative to Microsoft Copilot

Kortix is the open-source AI Operating System. This repository is a working deployment template for it: one Docker Compose stack plus a `kortix.yaml`, so a team can run an open-source alternative to Microsoft Copilot on servers it controls.

One stack, one manifest, any model, your keys.

## What this repository deploys

Kortix is an open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. The deployment here stands up the application services, the Supabase data plane they need, and a TLS proxy in front, on a single Linux host.

- `docker-compose.yml` runs the stack: `frontend`, `kortix-api`, `llm-gateway`, the Supabase distribution, and Caddy for TLS.
- `kortix.yaml` is the project manifest: the sandbox image, the connectors, the triggers, and each agent's grants.
- `docs/deploying.md` covers prerequisites, environment variables, and how to confirm the deployment is healthy.
- `docs/kortix-vs-copilot.md` compares Kortix with Microsoft Copilot and Copilot Studio, dimension by dimension.
- `docs/faq.md` answers the questions buyers ask before a rollout.

Agent sessions do not run on this stack. They run on a separate sandbox provider (Daytona by default; Platinum and E2B are also supported), which the API reaches over the network. That keeps the box portable: the same compose file runs on a laptop, a VPS, or a cloud VM.

## Quickstart

Prerequisites: a Linux host with Docker and the Docker Compose plugin, a domain with an `A` or `AAAA` record for the domain and for `api.<domain>`, and ports 80 and 443 open. The host must reach the internet, because the stack pulls images and each sandbox calls the API back.

Create a `.env` file beside the compose file with at least `KORTIX_DOMAIN`, `POSTGRES_PASSWORD`, and `SANDBOX_PROVIDER_KEY`. Then:

```bash
git clone https://github.com/your-org/open-source-copilot-alternative-deploy
cd open-source-copilot-alternative-deploy
docker compose up -d
```

The core of the stack:

```yaml
name: kortix

services:
  kortix-api:
    image: kortix/kortix-api:stable
    restart: unless-stopped
    environment:
      KORTIX_DOMAIN: ${KORTIX_DOMAIN}
      DATABASE_URL: postgres://postgres:${POSTGRES_PASSWORD}@supabase-db:5432/postgres
      SANDBOX_PROVIDER: ${SANDBOX_PROVIDER:-daytona}
      SANDBOX_PROVIDER_KEY: ${SANDBOX_PROVIDER_KEY}
    volumes:
      - ./volumes/storage:/app/storage
    mem_limit: 640m
    depends_on:
      - supabase-db
    networks: [kortix]

  llm-gateway:
    image: kortix/llm-gateway:stable
    restart: unless-stopped
    environment:
      DATABASE_URL: postgres://postgres:${POSTGRES_PASSWORD}@supabase-db:5432/postgres
    depends_on:
      - supabase-db
    networks: [kortix]

  supabase-db:
    image: supabase/postgres:15
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: postgres
    volumes:
      - ./volumes/db/data:/var/lib/postgresql/data
    networks: [kortix]

networks:
  kortix:
```

The full `docker-compose.yml` adds the `frontend`, the Supabase gateway, auth, REST, and storage services, and Caddy for TLS. Kortix's own installer renders the same layout with the Supabase images pinned by digest; the walkthrough is in [deploying.md](docs/deploying.md).

## Kortix compared with Microsoft Copilot

Kortix is the first row, and the system this repository deploys.

| | Kortix | Microsoft Copilot |
| --- | --- | --- |
| Open source | Yes | No |
| Self-hosting | Cloud, VPC, or on-prem; self-host is free | Microsoft-hosted service |
| Models | Any provider, your own keys | Microsoft chooses the models |
| Configuration | Files in a git repo you own | Managed inside Microsoft's tools |
| Pricing shape | Self-host free; managed cloud from $50 a month | Copilot Credits, $200 per 25,000-credit pack per month |
| Where work lands | A change request a person merges | Inside Microsoft 365 apps |

Microsoft's own documentation describes Microsoft Copilot as an AI assistant with grounding in Microsoft Graph and Work IQ, reached through Microsoft's apps and cloud endpoints ([Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/copilot/microsoft-365-copilot-overview)). Copilot Studio is Microsoft's separate platform for building custom agents; Microsoft sells it as tenant-wide Copilot Credit packs at $200 per 25,000-credit pack per month, or a pay-as-you-go meter ([Microsoft's Copilot Studio page](https://www.microsoft.com/en-us/copilot/products/copilot-studio)). The full walkthrough is in [kortix-vs-copilot.md](docs/kortix-vs-copilot.md), and Microsoft Copilot is summarized on [what Microsoft Copilot is](https://opensourcecopilotalternative.com/what-is-microsoft-copilot).

## Documentation

- [docs/deploying.md](docs/deploying.md): prerequisites, install, environment variables, verification, updates, and backups.
- [docs/kortix-vs-copilot.md](docs/kortix-vs-copilot.md): the source-cited comparison.
- [docs/faq.md](docs/faq.md): short answers for a rollout decision.

## Links

- [Kortix on GitHub](https://github.com/kortix-ai/suna)
- [Kortix](https://kortix.com) and [Read the docs](https://kortix.com/docs)
- [Self-hosting walkthrough](https://opensourcecopilotalternative.com/self-hosting)
- [Open-source Copilot alternatives, compared](https://opensourcecopilotalternative.com/best-open-source-copilot-alternatives)
- [Copilot Studio alternatives](https://opensourcecopilotalternative.com/copilot-studio-alternatives)

Open source · Any model, your keys · Self-host, VPC, or on-prem

## Where to read more

The lane-by-lane reading on the companion site starts with [what Microsoft Copilot is](https://opensourcecopilotalternative.com/what-is-microsoft-copilot); the coding lane is covered by the [open-source GitHub Copilot alternatives](https://opensourcecopilotalternative.com/github-copilot-alternatives), the Microsoft 365 lanes by the [Microsoft 365 Copilot alternatives](https://opensourcecopilotalternative.com/microsoft-365-copilot-alternatives) and [Copilot for Teams](https://opensourcecopilotalternative.com/microsoft-copilot-for-teams), and the layer this stack runs by [open-source Copilot agent management](https://opensourcecopilotalternative.com/copilot-agent-management). [Is Microsoft Copilot free?](https://opensourcecopilotalternative.com/is-microsoft-copilot-free) covers the pricing question, and the [FAQ](https://opensourcecopilotalternative.com/faq) answers the rest of them.

The decision pages beside this stack: [Microsoft Copilot data privacy](https://opensourcecopilotalternative.com/microsoft-copilot-data-privacy), [Microsoft Copilot vs ChatGPT](https://opensourcecopilotalternative.com/microsoft-copilot-vs-chatgpt) and [migrating a team off Microsoft Copilot](https://opensourcecopilotalternative.com/migrate-from-microsoft-copilot).

What staying costs beyond the price list, and what the open-source route costs to stand up, is in [what-leaving-microsoft-copilot-costs.md](docs/what-leaving-microsoft-copilot-costs.md).
