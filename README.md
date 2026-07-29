# Deploy and Host Readeck on Railway

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/readeck-1)

**Published on the Railway marketplace:** https://railway.com/deploy/readeck-1

Readeck is an open-source read-later and bookmark manager that saves readable copies of web pages. It organizes articles with labels, favorites, archives, collections, highlights, full-text search, EPUB export, and browser extensions while keeping the reading library on infrastructure controlled by the deployer.

## About Hosting Readeck

Hosting Readeck requires one public container and one persistent volume. This package builds a minimal wrapper from the digest-pinned, Umbrel-tested Readeck 0.22.3 image, exposes its documented port `8000` through Railway HTTPS, and mounts `/readeck`, which contains `config.toml`, the SQLite database, custom content scripts, and archived bookmark files. Railway supplies the canonical public URL and checks the unauthenticated `/login` page for readiness. A generated application secret protects sessions, while a generated password creates the initial `admin` account before the server first becomes public. SQLite is suitable for a single Readeck instance; do not add replicas that share this volume.

> **Lawful personal archiving notice:** Save only pages you are authorized to access and preserve. Follow each site's terms and applicable copyright law; do not circumvent access controls or paywalls, and do not redistribute archived content without permission.

## Common Use Cases

- Keeping a private read-later library with offline article copies and full-text search
- Organizing research with labels, collections, favorites, highlights, and annotations
- Exporting saved articles and collections to EPUB for personal reading

## Dependencies for Readeck Hosting

- `codeberg.org/readeck/readeck:0.22.3`
- This repository's minimal Dockerfile wrapper, built by Railway from GitHub
- One Railway persistent volume mounted at `/readeck`
- One Railway-generated HTTPS service domain targeting port `8000`

### Deployment Dependencies

- [Readeck installation documentation](https://readeck.org/en/docs/)
- [Readeck configuration reference](https://readeck.org/en/docs/configuration)
- [Readeck command reference](https://readeck.org/en/docs/commands)
- [Readeck Docker Compose examples](https://readeck.org/en/docs/compose)
- [Readeck 0.22.3 release](https://codeberg.org/readeck/readeck/releases/tag/0.22.3)
- [Umbrel Readeck package](https://github.com/getumbrel/umbrel-apps/tree/master/readeck)

## Implementation Details

| Service | Image | Networking and health | Persistent storage |
| --- | --- | --- | --- |
| `Readeck` | GitHub Dockerfile wrapper based on `codeberg.org/readeck/readeck:0.22.3` at its verified digest | Public port `8000`; canonical Railway HTTPS URL; readiness path `/login` | `/readeck` for configuration, SQLite, archived pages, images, and content scripts |

The Dockerfile changes no Readeck binary, command, environment, or feature. Its `FROM` is pinned to the verified upstream digest, and its only additional instruction materializes the inherited `/readeck` working directory required by Railway before a volume is mounted.

The startup command uses Readeck's documented `user` command to create an administrator only when that username does not already exist. Retrieve `READECK_ADMIN_PASSWORD` from Railway service variables, sign in as `admin`, then change the password in Readeck; subsequent restarts do not overwrite it. Keep Railway variable access restricted and enable TOTP in the account security settings. Railway volumes preserve state across deployments but are not independent backups, so use Readeck's documented export and backup procedures.

Readeck 0.22.3 exposes a container CLI health check rather than a dedicated public HTTP health route. Railway therefore uses `/login`, an unauthenticated application page that returns HTTP `200` only after the HTTP server is ready.

This is a community-maintained deployment package and does not imply affiliation with or endorsement by Readeck.

## Why Deploy Readeck on Railway?

Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying Readeck on Railway, you are one step closer to supporting a complete full-stack application with minimal burden. Host your servers, databases, AI agents, and more on Railway.
