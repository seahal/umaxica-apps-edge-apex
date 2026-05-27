![GitHub last commit (branch)](https://img.shields.io/github/last-commit/seahal/umaxica-apps-edge/main)

# Umaxica Apps Edge Apex

（ ＾ν＾） Hello, World!

## Prerequisites

- Node.js 26.x (`node:26-trixie`)
- [pnpm](https://pnpm.io/) 10.29+
- Docker & Docker Compose (optional)

## Workspaces

| Package | Role                | Domain        | Dev Port |
| ------- | ------------------- | ------------- | -------- |
| `com`   | Apex/static worker  | `umaxica.com` | 5101     |
| `app`   | Apex/static worker  | `umaxica.app` | 5401     |
| `org`   | Apex/static worker  | `umaxica.org` | 5301     |
| `net`   | Network apex worker | —             | 5201     |

## Quick Start

```bash
vp install

# Run a specific workspace
vp run --filter <workspace> server   # e.g. com, app

# Docker (optional)
docker compose up && docker compose exec apex bash
```

## Scripts

Toolchain is [Vite+](https://vite.plus/) (`vp` CLI).

```bash
vp check                  # format + lint + type-check
vp fmt                    # oxfmt
vp lint                   # oxlint
vp test                   # vitest
vp run --filter <ws> <script>
```

## Development Environment

### Toolchain

| Tool                                                            | Role                                      | Version |
| --------------------------------------------------------------- | ----------------------------------------- | ------- |
| [Vite+](https://vite.plus/) (`vp`)                              | Unified CLI (dev, build, test, lint, fmt) | 0.1.x   |
| [pnpm](https://pnpm.io/)                                        | Package manager (wrapped by `vp`)         | 10.32.x |
| [Oxfmt](https://oxc.rs/)                                        | Formatter (`vp fmt`)                      | bundled |
| [Oxlint](https://oxc.rs/)                                       | Linter (`vp lint`)                        | bundled |
| [tsgo](https://github.com/nicolo-ribaudo/tsgo)                  | Type checker (`vp check`)                 | bundled |
| [Vitest](https://vitest.dev/)                                   | Test runner (`vp test`)                   | bundled |
| [Wrangler](https://developers.cloudflare.com/workers/wrangler/) | Cloudflare Workers CLI                    | 4.x     |

Oxfmt, Oxlint, tsgo, and Vitest are bundled with Vite+ — no separate installation required.

### Docker / DevContainer

The development environment can be set up via Docker + VS Code DevContainer.

- **Base image**: `node:26-trixie` with pnpm (corepack) and Vite+ pre-installed
- **DevContainer**: configured in `.devcontainer/devcontainer.json`
  - Extensions: Claude Code, Oxc, Playwright
  - Disabled: ESLint, Prettier, GitLens, GitHub Copilot
  - Security: Trivy, Gitleaks (via pre-commit hooks)

```bash
# VS Code: use "Reopen in Container" for automatic setup

# Or start manually with Docker Compose
docker compose up && docker compose exec apex bash
```

## Production Environment

| Platform                                              | Workspaces                 | Domains                                     |
| ----------------------------------------------------- | -------------------------- | ------------------------------------------- |
| [Cloudflare Workers](https://workers.cloudflare.com/) | `com`, `app`, `org`, `net` | `umaxica.com`, `umaxica.app`, `umaxica.org` |

### Deployment

```bash
vp run --filter <workspace> deploy            # direct deploy
vp run --filter <workspace> deploy:upload      # versioned: upload then promote
vp run --filter <workspace> deploy:promote
```

### Environment Variables

Cloudflare workspaces use `wrangler.jsonc` (`vars` + environments).

For local Docker Compose development, use the Apex worker ports listed in the workspace table.

## Monitoring

Uptime checks via [Pulsetic](https://pulsetic.com/).

## Acknowledgement

- Public availability of this repository is not guaranteed permanently.
