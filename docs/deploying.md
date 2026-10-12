# Deploy Kortix, the open-source AI Operating System

Kortix is the open-source AI Operating System, and the deployment in this repository runs it on one Linux host. This walkthrough covers prerequisites, the install, the environment variables that matter, how to confirm the deployment is healthy, and how updates and backups work.

Kortix runs as one Docker Compose stack: the `frontend`, `kortix-api`, the `llm-gateway`, and a Supabase distribution for auth, data, and file storage. A Caddy service in front handles TLS. Agent sessions do not run on this host: they run on a separate sandbox provider the API reaches over the network.

## Prerequisites

- A Linux host, `x86_64`, with Docker Engine and the Docker Compose plugin. Kortix's one-shot bootstrap is Linux only; on another operating system, install the CLI and use the manual path.
- 8 GiB of RAM is the baseline for the default 640 MiB `kortix-api` memory limit. Use 16 GiB if you raise the limit.
- A domain with an `A` or `AAAA` record for the domain and for `api.<domain>`, both pointing at the host IP. The Caddy service uses ports 80 and 443 to issue a TLS certificate through ACME, so open both.
- Outbound internet access. The stack pulls images from `docker.io`, and every sandbox calls the API back at the domain. This is not an air-gapped deployment.
- A sandbox provider key. Daytona is the default; Platinum and E2B are also supported.
- Model credentials. Any model provider with your own keys, the ChatGPT plan you already pay for, or sign in with your OpenCode Console account for OpenCode Zen and Go. A self-hosted instance uses your own key by default.

## Install

The canonical path uses the Kortix CLI:

```bash
curl -fsSL https://kortix.com/install | bash
kortix self-host init --domain kortix.example.com
kortix self-host start
```

`kortix self-host init` renders a `docker-compose.yml` and a `.env` into `~/.config/kortix/self-host/<instance>/`, and adds a `Caddyfile` and an updater when you set a domain. `kortix self-host start` runs `docker compose up`. The same artifact runs on a laptop, a VPS, or a cloud VM; the domain is only the `KORTIX_DOMAIN` value, not a different setup.

To run the template in this repository directly:

```bash
git clone https://github.com/your-org/open-source-copilot-alternative-deploy
cd open-source-copilot-alternative-deploy
docker compose up -d
```

For a first look without a domain, use a Cloudflare tunnel instead:

```bash
kortix self-host init --tunnel cloudflare
kortix self-host start
```

The tunnel URL changes on every restart, so use this mode for evaluation only. Sessions cannot run without a domain or a tunnel, because the sandbox needs a stable URL to reach the API.

## Environment variables

| Variable | What it sets |
| --- | --- |
| `KORTIX_DOMAIN` | The public hostname Caddy issues a certificate for. |
| `POSTGRES_PASSWORD` | The password for the bundled Postgres database. |
| `SANDBOX_PROVIDER` | Which sandbox provider runs sessions. `daytona` by default; `platinum` and `e2b` are also supported. |
| `SANDBOX_PROVIDER_KEY` | The provider key, set with `kortix self-host configure`. |
| `ANTHROPIC_API_KEY`, `OPENAI_API_KEY` | Bring-your-own model keys, used by the LLM gateway. Set the ones you use. |
| `KORTIX_API_MEMORY_LIMIT` | The API memory limit. Defaults to `640m`; raise it to `1024m` on a 16 GiB host when API traffic reaches the default. |
| `KORTIX_UPDATE_TIME`, `KORTIX_UPDATE_TZ` | When the updater checks for a new image. Default `02:00` in `America/New_York`. |

## Verify the deployment

1. Run `kortix self-host status`, then `kortix self-host logs` and `kortix self-host doctor` while the stack settles.
2. Run `docker compose ps` and confirm every service reports healthy.
3. Run `docker stats --no-stream` and confirm the API memory limit is the one you set.
4. Open `https://<KORTIX_DOMAIN>` and sign in.
5. In the dashboard, connect GitHub under Settings → Git, so sessions can clone the project repository.
6. Add an LLM key in the model picker.
7. Start a session. The agent runs on the sandbox provider, not on this host, and each session gets its own cloud computer and branch.
8. Confirm the governance path: when the agent is done, it opens a change request. Merge is default-deny for agents, so a person reviews the diff before anything lands on `main`.

## Updates

Every instance updates itself automatically once a day. To pin an exact version instead, run `kortix self-host update --tag 0.9.84`. To turn the updater off, pass `--auto-update off`. An instance tracks one of two channels: `stable` (the default, promoted by hand) or `latest` (every production release), and an explicit pin overrides either.

## Backups

Kortix has no separate backup system. Each instance stores its data as two directories under `~/.config/kortix/self-host/<instance>/`: `volumes/db/data` for Postgres and `volumes/storage` for file storage. The instance's `.env` holds every secret and signing key it uses. Back up all three before you run a destructive command. `kortix self-host uninstall` runs `docker compose down --volumes --remove-orphans` and deletes the instance directory, which removes both bind mounts.

## Next steps

- The live [self-hosting walkthrough](https://opensourcecopilotalternative.com/self-hosting) summarizes the same three commands and what the first run asks.
- [kortix-vs-copilot.md](kortix-vs-copilot.md) compares Kortix with Microsoft Copilot and Copilot Studio.
- [faq.md](faq.md) answers the rollout questions buyers ask.
