# Readeck Railway Template — Source Record and Manual Publication Package

**Status: DRAFT CREATION FAILED — SAFE GATE STOPPED AUTOMATED PUBLICATION.** The verified source service deploys the exact requested Codeberg image, but `railway templates create` rejected that source with `Invalid Docker image for Readeck`. No draft identity was returned, no serialized template exists to reconcile, and no publication command was run. Publication will be performed manually.

## Approved Identity and Marketplace Metadata

| Item | Value |
| --- | --- |
| Exact project and template title | `Readeck` |
| Draft template ID / code | Not created; Railway rejected the Codeberg image during draft generation |
| Status | `UNPUBLISHED`; safe gate blocked |
| Category | `Other` |
| Description | `Save, organize, and preserve web articles for focused reading.` |
| Overview | [README.md](./README.md) |
| Official icon | https://codeberg.org/readeck/readeck/raw/tag/0.22.3/web/media/logo-square.svg |
| Workspace | Agastya Fauzan Alfath's Projects (`017da116-eee8-4131-a3a0-2f2d3de455f5`) |
| Source project | `Readeck` (`938d1012-731c-42d8-ad28-755b690ad063`) |
| Source environment | `production` (`c85c83cc-e082-4c30-b93a-68e35433017b`) |
| Source service | `Readeck` (`5e2601cb-ef75-408d-94c4-e734d64998f7`) |
| Verified source URL | https://readeck-production-5d4e.up.railway.app |

The icon is Readeck's official versioned `logo-square.svg`: HTTP `200`, SVG, `240 × 240`, square view box, with transparent rounded corners suitable for both the template and service. Readeck remains the upstream software author. This is a community-maintained template and does not imply affiliation with or endorsement by Readeck.

## Verified Source Service Graph

| Service | Image | Networking and health | Persistent volume | Verified deployment |
| --- | --- | --- | --- | --- |
| `Readeck` | `codeberg.org/readeck/readeck:0.22.3` (resolved digest `sha256:ed1c513f8e1d59b1d38fda324ac279eeb65afd0327907e498061ec9fd2f31c15`) | Public target port `8000`; canonical Railway HTTPS URL; `/login`; 300-second timeout; `ON_FAILURE`, 10 retries | `readeck-volume` (`21ef316f-3624-412d-98c3-21f3000b3055`) at `/readeck` | `30d64fd6-e453-441f-bd99-d3d68cb5b211` — `SUCCESS` |

The final source startup command is:

```text
/bin/sh -c 'set -eu; if /bin/readeck user -u "$READECK_ADMIN_USERNAME" -p env:READECK_ADMIN_PASSWORD -group admin -dry-run -json | grep -q "\"exists\":false"; then /bin/readeck user -u "$READECK_ADMIN_USERNAME" -p env:READECK_ADMIN_PASSWORD -group admin >/dev/null; fi; exec /bin/readeck serve'
```

Readeck 0.22.3's documented `user` command reads the password from the named environment variable. The dry run prevents startup from resetting a password changed in the UI, while a fresh volume receives an administrator before the public server starts. `READECK_SECRET_KEY` and `READECK_ADMIN_PASSWORD` were rotated after smoke testing; neither active source value appears in this repository.

## Upstream and Umbrel Contract Evidence

- Umbrel pins `codeberg.org/readeck/readeck:0.22.3` at the same digest, listens on `0.0.0.0:8000`, mounts all state at `/readeck`, sets a persistent secret key, and uses `/bin/readeck healthcheck -config config.toml` inside the container.
- Umbrel's manifest names Readeck 0.22.3, describes a read-later and archival application, and routes its proxy to container port `8000`.
- Readeck's Compose guide documents port `8000`, `/readeck` persistence, `READECK_SERVER_HOST`, `READECK_SERVER_PORT`, log variables, and the same CLI health check.
- Readeck's configuration reference documents `READECK_SERVER_BASE_URL` as the canonical instance URL, `READECK_SECRET_KEY` as a required long random secret, and the default SQLite database at `data/db.sqlite3` below the working directory.
- Readeck's command reference documents user creation/update, admin groups, JSON dry runs, and `env:VARIABLE` password mapping for automated deployments.
- Readeck 0.22.3 has no dedicated unauthenticated public HTTP health route. Railway requires HTTP `200`, so the source uses `/login`, which remained `200` before and after restart and proves the application HTTP stack is ready.

## Source Verification and Cleanup Evidence

- Final deployment `30d64fd6-e453-441f-bd99-d3d68cb5b211` reached `SUCCESS` with the requested image, canonical URL reference, startup command, `/login` health path, restart policy, public domain, and `/readeck` volume.
- The public `/login` UI returned HTTP `200`; administrator authentication reached the protected profile and bookmark interface.
- The smoke test supplied its own short HTML document for the reserved `https://example.com/railway-readeck-smoke-test` URL. It did not scrape a third-party publication or bypass access controls.
- Readeck accepted the page, completed extraction with state `0`, and returned the supplied title. The same bookmark remained readable after a Railway service restart, proving SQLite and archived content persisted on `/readeck`.
- The smoke bookmark was deleted and a follow-up request returned HTTP `404`.
- The temporary verification password is no longer active. Both source secrets were rotated without printing them, the protected account was updated, the final conditional bootstrap was restored, and authentication with the rotated Railway variable succeeded.
- The lawful-use notice in [README.md](./README.md) tells deployers to follow site terms and copyright law, avoid circumvention, and avoid unauthorized redistribution.

## Draft Attempt, Audit Result, and Safe Gate

The exact command `railway templates create --project 938d1012-731c-42d8-ad28-755b690ad063 --environment c85c83cc-e082-4c30-b93a-68e35433017b --json` reached Railway's project and environment selection, then returned `Invalid Docker image for Readeck`.

Because Railway returned no draft:

- there is no template ID, code, editor URL, or `serializedConfig` to audit;
- the source project's live secret values cannot be safely replaced with template functions in an editor;
- the Codeberg image must not be silently replaced by a mirror or wrapper image, because the approved scope requires the exact upstream image;
- `railway templates publish` was not attempted, because no reconciled draft passed the variable and source-secret gate.

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

1. Ask Railway support or use the template composer to create an unpublished draft that accepts exact image `codeberg.org/readeck/readeck:0.22.3`; do not substitute another registry or image.
2. Preserve exact service namespace `Readeck`, public port `8000`, `/login` health path with 300-second timeout, `/readeck` volume, startup command, and `ON_FAILURE` policy with 10 retries.
3. Paste the complete raw block, keep all nine variables required, and enter every inventory description. Confirm only the two secret variables use generators and that the canonical URL remains a Railway reference.
4. Set the exact title, category, description, official icon, service description, and [README.md](./README.md) overview.
5. Reconcile the complete serialized graph key-for-key and prove that no active source password or secret key appears.
6. Deploy the draft into a disposable project. Verify generated secrets, no avoidable prompts, successful health and UI, protected first login, supplied-HTML bookmark creation, persistence across restart, test-content deletion, and secret rotation.
7. Only after an explicit safe-gate approval, publish and then confirm the returned marketplace code, metadata, graph, and HTTP `200` deploy URL.
8. The manual publisher owns editor reconciliation, publication approval, disposable-resource cleanup, and any later source-project teardown.

## Residual Risks

- Railway currently rejects the exact Codeberg image during draft generation even though the source runtime deploys it successfully. Registry acceptance is the publication blocker.
- Readeck uses SQLite on the mounted volume. Keep one replica, avoid network filesystems, and maintain independent exports or backups.
- Railway `/login` readiness confirms the HTTP application is serving but is less comprehensive than Readeck's container CLI health check.
- The initial password remains readable to people with Railway variable access. Change it after first login, restrict project membership, and enable TOTP.
- Archived pages may contain copyrighted or sensitive material. Deployers remain responsible for lawful access, storage, sharing, and compliance with site terms.
