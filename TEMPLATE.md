# Readeck Railway Template — Source Record and Manual Publication Package

**Status: UNPUBLISHED — SAFE GATE STOPPED AUTOMATED PUBLICATION.** The minimal GitHub-source recovery removed Railway's `Invalid Docker image` blocker and produced a project-derived draft. The draft preserved the verified service graph and canonical URL reference, but stripped eight required defaults and omitted the 300-second health timeout. Publishing it now would produce a broken and unprotected deployment, so no publication command was run. Publication will be performed manually.

## Approved Identity and Marketplace Metadata

| Item | Value |
| --- | --- |
| Exact project and template title | `Readeck` |
| Draft template ID / code | `0ef8d34f-b56a-4031-80d0-91a86c55a2cb` / `LJorwr` |
| Editor URL | https://railway.com/workspace/templates/0ef8d34f-b56a-4031-80d0-91a86c55a2cb |
| Status | `UNPUBLISHED`; safe gate blocked |
| Category | `Other` |
| Description | `Save, organize, and preserve web articles for focused reading.` |
| Overview | [README.md](./README.md) |
| Official icon | https://codeberg.org/readeck/readeck/raw/tag/0.22.3/web/media/logo-square.svg |
| Workspace | Agastya Fauzan Alfath's Projects (`017da116-eee8-4131-a3a0-2f2d3de455f5`) |
| Source project | `Readeck` (`938d1012-731c-42d8-ad28-755b690ad063`) |
| Source environment | `production` (`c85c83cc-e082-4c30-b93a-68e35433017b`) |
| Source service | `Readeck` (`e26ca3f3-6ac3-4c42-ac91-38d8d53b63a7`) |
| Verified source URL | https://readeck-production-96e3.up.railway.app |

The icon is Readeck's official versioned `logo-square.svg`: HTTP `200`, SVG, `240 × 240`, square view box, with transparent rounded corners suitable for both the template and service. Readeck remains the upstream software author. This is a community-maintained template and does not imply affiliation with or endorsement by Readeck.

## Verified Source Service Graph

| Service | Source and immutable base | Networking and health | Persistent volume | Verified deployment |
| --- | --- | --- | --- | --- |
| `Readeck` | `aalfath/readeck-railway-template` at commit `514079f`; Dockerfile `FROM codeberg.org/readeck/readeck:0.22.3@sha256:ed1c513f8e1d59b1d38fda324ac279eeb65afd0327907e498061ec9fd2f31c15` | Public target port `8000`; canonical Railway HTTPS URL; `/login`; 300-second timeout; `ON_FAILURE`, 10 retries | `readeck-volume` (`21ef316f-3624-412d-98c3-21f3000b3055`) at `/readeck` | `915fadff-4768-42e1-83df-58512213cb49` — `SUCCESS` |

The final source startup command is:

```text
/bin/sh -c 'set -eu; if /bin/readeck user -u "$READECK_ADMIN_USERNAME" -p env:READECK_ADMIN_PASSWORD -group admin -dry-run -json | grep -q "\"exists\":false"; then /bin/readeck user -u "$READECK_ADMIN_USERNAME" -p env:READECK_ADMIN_PASSWORD -group admin >/dev/null; fi; exec /bin/readeck serve'
```

Readeck 0.22.3's documented `user` command reads the password from the named environment variable. The dry run prevents startup from resetting a password changed in the UI, while a fresh volume receives an administrator before the public server starts. `READECK_SECRET_KEY` and `READECK_ADMIN_PASSWORD` were rotated after smoke testing; neither active source value appears in this repository.

The recovery Dockerfile is intentionally limited to the exact digest-pinned `FROM` plus `RUN mkdir -p /readeck`. The second instruction only materializes the inherited working directory because Railway validates it before mounting a volume. Local and Railway builds confirmed that the wrapper inherits Readeck's binary and default command unchanged. Railway built commit `514079f` with the Dockerfile builder and produced wrapper digest `sha256:ef2fb5ebbd496355c8d8fc1c878f50d4c10293b810569e63e29e965bfe9c73b6`.

## Upstream and Umbrel Contract Evidence

- Umbrel pins `codeberg.org/readeck/readeck:0.22.3` at the same digest, listens on `0.0.0.0:8000`, mounts all state at `/readeck`, sets a persistent secret key, and uses `/bin/readeck healthcheck -config config.toml` inside the container.
- Umbrel's manifest names Readeck 0.22.3, describes a read-later and archival application, and routes its proxy to container port `8000`.
- Readeck's Compose guide documents port `8000`, `/readeck` persistence, `READECK_SERVER_HOST`, `READECK_SERVER_PORT`, log variables, and the same CLI health check.
- Readeck's configuration reference documents `READECK_SERVER_BASE_URL` as the canonical instance URL, `READECK_SECRET_KEY` as a required long random secret, and the default SQLite database at `data/db.sqlite3` below the working directory.
- Readeck's command reference documents user creation/update, admin groups, JSON dry runs, and `env:VARIABLE` password mapping for automated deployments.
- Readeck 0.22.3 has no dedicated unauthenticated public HTTP health route. Railway requires HTTP `200`, so the source uses `/login`, which remained `200` before and after restart and proves the application HTTP stack is ready.

## Source Verification and Cleanup Evidence

- Replacement deployment `915fadff-4768-42e1-83df-58512213cb49` reached `SUCCESS` from GitHub commit `514079f` with the exact digest-pinned base image, canonical URL reference, startup command, `/login` health path, 300-second timeout, restart policy, public domain, and existing `/readeck` volume.
- The original image-source service was first renamed and stopped. The existing volume was detached only after the replacement wrapper built and served HTTP successfully, then attached to the replacement at the same `/readeck` mount. The superseded service was deleted only after the complete replacement verification passed; the project now contains one service named exactly `Readeck`.
- The public `/login` UI returned HTTP `200`; administrator authentication reached the protected profile and bookmark interface.
- The replacement authenticated with the administrator stored on the migrated volume, proving the prior account and database survived the service swap. Its smoke test supplied its own short HTML document for the reserved `https://example.com/railway-readeck-replacement-smoke-test` URL; it did not scrape a third-party publication or bypass access controls.
- Readeck accepted the page, completed extraction with state `0`, and returned the supplied title. The same bookmark remained readable after a Railway service restart, proving SQLite and archived content persisted on `/readeck`.
- The smoke bookmark was deleted and a follow-up request returned HTTP `404`.
- The temporary verification password is no longer active. Both source secrets were rotated without printing them, the protected account was updated, the final conditional bootstrap was restored, and authentication with the rotated Railway variable succeeded.
- The lawful-use notice in [README.md](./README.md) tells deployers to follow site terms and copyright law, avoid circumvention, and avoid unauthorized redistribution.

## Draft Attempt, Audit Result, and Safe Gate

The exact command `railway templates create --project 938d1012-731c-42d8-ad28-755b690ad063 --environment c85c83cc-e082-4c30-b93a-68e35433017b --json` created draft `0ef8d34f-b56a-4031-80d0-91a86c55a2cb`, code `LJorwr`, title `Readeck`, status `UNPUBLISHED`, in the approved workspace.

The audited `serializedConfig` contains:

- one service named exactly `Readeck`, sourced from `https://github.com/aalfath/readeck-railway-template`;
- the complete conditional bootstrap, `/login` health path, `ON_FAILURE` policy with 10 retries, one public service-domain marker, and one `/readeck` volume;
- all nine expected variable keys, all required (`isOptional: false`);
- the exact canonical URL default `https://${{Readeck.RAILWAY_PUBLIC_DOMAIN}}`;
- **no default value for the other eight required variables**, including both credentials and all safe port, host, log, and administrator literals;
- no 300-second health timeout in the serialized deployment settings.

The gate therefore failed. No active source credential appears in the draft, but both generated template functions and six required safe literals are absent. `railway templates publish` was not run.

Publication will be performed manually.

## Raw Variable Block — Exact Namespace `Readeck`

```text
PORT=8000
READECK_SERVER_HOST=0.0.0.0
READECK_SERVER_PORT=8000
READECK_LOG_LEVEL=info
READECK_LOG_FORMAT=text
READECK_SERVER_BASE_URL=https://${{Readeck.RAILWAY_PUBLIC_DOMAIN}}
READECK_ADMIN_USERNAME=admin
READECK_ADMIN_PASSWORD=${{secret(48, "abcdef0123456789")}}
READECK_SECRET_KEY=${{secret(64, "abcdef0123456789")}}
```

## Variable Inventory

| Key | Description | State | Manual publisher instruction |
| --- | --- | --- | --- |
| `PORT` | Port Railway uses for deployment health checks and the public service target. | Required; safe literal | Set exactly `8000`. |
| `READECK_SERVER_HOST` | Interface on which Readeck accepts Railway proxy traffic. | Required; safe literal | Set exactly `0.0.0.0`. |
| `READECK_SERVER_PORT` | Readeck's documented HTTP listener port. | Required; safe literal | Set exactly `8000` and keep it aligned with `PORT` and the domain target. |
| `READECK_LOG_LEVEL` | Application logging threshold. | Required; safe literal | Set exactly `info`. |
| `READECK_LOG_FORMAT` | Human-readable container log format. | Required; safe literal | Set exactly `text`. |
| `READECK_SERVER_BASE_URL` | Canonical HTTPS URL used for links, cookies, redirects, and integrations. | Required; Railway reference | Set exactly `https://${{Readeck.RAILWAY_PUBLIC_DOMAIN}}` after preserving the exact service name. |
| `READECK_ADMIN_USERNAME` | Initial administrator identity checked by the idempotent bootstrap. | Required; safe literal | Set exactly `admin`; after first login the deployer may create another administrator before removing it. |
| `READECK_ADMIN_PASSWORD` | Strong first-login password created once on a fresh volume. | Required; generated secret | Set the 48-character hexadecimal template function; retrieve it from Railway, sign in, change it in Readeck, and keep variable access restricted. |
| `READECK_SECRET_KEY` | Long-lived key protecting application sessions and other signed data. | Required; generated secret | Set the 64-character hexadecimal template function exactly once; never change it on an initialized deployment. |

No deployer input is required. Railway can generate both secrets, the canonical URL is derived from the exact service namespace, and every other value is a portable upstream-supported literal.

## Icon and Copy Package

- Template and service icon: `https://codeberg.org/readeck/readeck/raw/tag/0.22.3/web/media/logo-square.svg`.
- Verification: HTTP `200`, official versioned SVG, square `240 × 240` canvas, transparency-suitable rounded corners.
- Attribution: official Readeck repository asset; Readeck is the upstream project.
- Affiliation caveat: community-maintained template; do not claim endorsement or official publisher status.
- Exact title: `Readeck`.
- Exact category: `Other`.
- Exact short description: `Save, organize, and preserve web articles for focused reading.`
- Copy-ready overview: [README.md](./README.md).
- Service description if requested: `Readeck 0.22.3 read-later server with persistent SQLite and archived web content.`

## Remaining Manual Publication Actions

1. Open https://railway.com/workspace/templates/0ef8d34f-b56a-4031-80d0-91a86c55a2cb and preserve exact service namespace `Readeck` and the GitHub Dockerfile source.
2. Preserve public port `8000`, restore the `/login` health path's 300-second timeout, retain the `/readeck` volume, startup command, and `ON_FAILURE` policy with 10 retries.
3. Paste the complete raw block, keep all nine variables required, and enter every inventory description. Confirm only the two secret variables use generators, the canonical URL remains the existing Railway reference, and no source credential is entered.
4. Set the exact title, category, description, official icon, service description, and [README.md](./README.md) overview.
5. Reconcile the complete serialized graph key-for-key and prove that no active source password or secret key appears.
6. Deploy the draft into a disposable project. Verify generated secrets, no avoidable prompts, successful health and UI, protected first login, supplied-HTML bookmark creation, persistence across restart, test-content deletion, and secret rotation.
7. Only after an explicit safe-gate approval, publish and then confirm the returned marketplace code, metadata, graph, and HTTP `200` deploy URL.
8. The manual publisher owns editor reconciliation, publication approval, disposable-resource cleanup, and any later source-project teardown.

## Residual Risks

- The image-registry blocker is resolved by the minimal GitHub Dockerfile wrapper, but the generated draft strips eight required defaults and one health timeout; manual editor reconciliation is the current publication blocker.
- The template source follows this repository's GitHub branch. Future repository changes can alter consumer builds even though the upstream Readeck base is digest-pinned; review and verify every wrapper commit before publishing or updating the template.
- Readeck uses SQLite on the mounted volume. Keep one replica, avoid network filesystems, and maintain independent exports or backups.
- Railway `/login` readiness confirms the HTTP application is serving but is less comprehensive than Readeck's container CLI health check.
- The initial password remains readable to people with Railway variable access. Change it after first login, restrict project membership, and enable TOTP.
- Archived pages may contain copyrighted or sensitive material. Deployers remain responsible for lawful access, storage, sharing, and compliance with site terms.
