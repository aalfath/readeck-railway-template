# Changelog

Changes to this Railway template, newest first. Breaking changes are listed first in each entry.

## 0.23.5 (2026-11-12)

**Breaking (upstream):** Readeck 0.23 removed Forwarded Authentication and replaced it with OpenID Connect. This template never configured Forwarded Authentication, so a default deployment is not affected. If you enabled it yourself, move to OIDC before applying this update ([Readeck 0.23 release notes](https://readeck.org/en/blog/202608-readeck-23/)).

- Fix: the template builds again. The Dockerfile now pins `codeberg.org/readeck/readeck:0.23.5@sha256:17dea468f4f713930338062f58a3a5e0d04e37d2961ddb827792e223f3db91b4`. Readeck removed the previously pinned 0.22.3 image from Codeberg's registry, so new deployments of the template failed to build.
- New in Readeck 0.23: OpenID Connect sign-in, a dashboard view, bookmark notes, a refreshed interface, a wider default list of IP ranges the extractor refuses to fetch, and updated CA certificates. See the [0.23.0 to 0.23.5 release notes](https://codeberg.org/readeck/readeck/releases).
- No change to variables, the start command, the `/login` health check or the `/readeck` volume. Readeck updates its SQLite database when it starts on a new version. An upgrade from 0.22.3 data could not be tested, because Codeberg no longer serves the 0.22.3 image, so back up `/readeck` (or export your bookmarks) before applying this update.
- README: the Deploy on Railway button now links to `railway.com/new/template/readeck-1`, and a note explains how updates reach deployers.

## 0.22.3 (2026-07-29)

- First published version, on Readeck 0.22.3.
