# renovate-yarn-inherited-private-packages-mvce

Minimal, self-contained repo to demonstrate Renovate PR #44199 behavior:
`feat(npm/yarn): Add support for loading config from inherited yarnrc.yml files`.

## What this repo demonstrates

- Root `.yarnrc.yml` config (inherited by workspace packages):
  - `npmScopes` for GitHub Packages (`npm.pkg.github.com`)
  - `npmMinimalAgeGate`
- Package-level `.yarnrc.yml` config in `packages/foo`:
  - `npmPreapprovedPackages`
- Consumer package depends on a scoped package published to GitHub Packages.

This mirrors the failing path where Renovate must correctly merge inherited Yarn config across directories.

## Repo layout

- `packages/private-lib`: package published to GitHub Packages as `@renovate-yarn-inherited-mvce/private-lib`
- `packages/foo`: consumer package depending on `@renovate-yarn-inherited-mvce/private-lib@0.0.1`

## Publish `private-lib` to GitHub Packages

1. Create GitHub repo under owner `renovate-yarn-inherited-mvce` scope compatibility, or adjust scope names in files.
2. Push this repo.
3. Run workflow `Publish private-lib to GitHub Packages` (manual dispatch), or push tag `private-lib-v0.0.1`.

Workflow file: `.github/workflows/publish-private-lib.yml`.

## Run Renovate locally against this repo

From your Renovate checkout:

```bash
cd ~/develop/renovate
LOG_LEVEL=debug pnpm start -- \
  --platform=local \
  --repository-cache=reset \
  --require-config=ignored \
  --onboarding=false \
  --local-dir=~/develop/renovate-yarn-inherited-private-packages-mvce
```

## Notes

- For local/private registry access, configure auth for `npm.pkg.github.com` as needed.
- If your GitHub owner is different, update the scope `renovate-yarn-inherited-mvce` in:
  - `.yarnrc.yml`
  - `packages/private-lib/package.json`
  - `packages/foo/package.json`
  - `packages/foo/.yarnrc.yml`
