---
name: astro_prepare
description: Initialize a new Astro Starlight website to be hosted on GitHub Pages.
---

Purpose: Create an Astro Starlight site skeleton that future steps will populate with documentation.

## STRICT RULES
- MUST read `specs/astro_site/parameters.md` to get the Site Name and GitHub Pages Repository (required)
- MUST clone the GitHub Pages Repository into the workspace if not present
- MUST run `npm --yes --cache ./.npm-cache create astro@latest web -- --template starlight --yes` inside the cloned repository root
- MUST be idempotent: if the repository already contains a valid Astro project, verify and continue without recreating
- MUST create `repos/` and `context/` folders at the workspace root if they don't exist
- MUST copy `specs/astro_site/deploy-site.yml` to `<siteDir>/.github/workflows/deploy-site.yml`
- NEVER ask any questions or require interactive input

## Preconditions
1. Load Parameters: Read `specs/astro_site/parameters.md` and extract:
   - Site Name (required)
   - GitHub Pages Repository (org/repo) — required (must already exist; no repository creation)
2. Ensure network access is available for `npm create astro@latest` to download the initializer.

## High-Level Workflow
1. Determine workspace root (current project workspace), then parse `org/repo` from the GitHub Pages Repository.
2. Compute `siteDir = <workspace>/<repo>` and `repoUrl = https://github.com/<org>/<repo>.git` (or use the provided URL/SSH form).
3. If `siteDir` does not exist, clone the repository into `siteDir`.
4. Inside `siteDir`:
   - If it already contains a valid Astro Starlight project (has `web/package.json` and `web/src/content/docs/`), verify and continue.
   - Otherwise, execute: `npm --yes --cache ./.npm-cache create astro@latest web -- --template starlight --yes`
5. Ensure directories `repos/` and `context/` exist at the workspace root.
6. Install GitHub Actions workflow: copy `specs/astro_site/deploy-site.yml` to `<siteDir>/.github/workflows/deploy-site.yml` (create directories as needed).

## Verification
- Confirm that `<workspace>/<repo>/web/package.json` exists.
- Confirm presence of Astro config file in `<workspace>/<repo>/web/` (e.g., `astro.config.*`) and `web/src/content/docs/` directory.
- Confirm `repos/` and `context/` directories are present at workspace root.
- Confirm workflow exists at `<workspace>/<repo>/.github/workflows/deploy-site.yml`.

## Example Output
```
Initializing Astro Starlight site...
✓ Cloned repo into ./<repo>
✓ Initialized Astro Starlight in ./<repo>/web
✓ Created ./repos and ./context folders
✓ Installed workflow at ./<repo>/.github/workflows/deploy-site.yml
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
