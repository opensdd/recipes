---
name: docusaurus_prepare
description: Initialize a new Docusaurus website to be hosted on GitHub Pages.
---

Purpose: Create a Docusaurus site skeleton that future steps will populate with documentation.

## STRICT RULES
- MUST read `specs/docusaurus_site/parameters.md` to get the Site Name and GitHub Pages Repository (required)
- MUST clone the GitHub Pages Repository into the workspace if not present
- MUST run `npx --cache ./.npm-cache create-docusaurus@latest web classic -t` inside the cloned repository root
- MUST be idempotent: if the repository already contains a valid Docusaurus project, verify and continue without recreating
- MUST create `repos/` and `context/` folders at the workspace root if they don't exist
- MUST copy `specs/docusaurus_site/deploy-site.yml` to `<siteDir>/.github/workflows/deploy-site.yml`
- NEVER ask any questions or require interactive input

## Preconditions
1. Load Parameters: Read `specs/docusaurus_site/parameters.md` and extract:
   - Site Name (required)
   - GitHub Pages Repository (org/repo) — required (must already exist; no repository creation)
2. Ensure network access is available for `npx` to download the Docusaurus initializer.

## High-Level Workflow
1. Determine workspace root (current project workspace), then parse `org/repo` from the GitHub Pages Repository.
2. Compute `siteDir = <workspace>/<repo>` and `repoUrl = https://github.com/<org>/<repo>.git` (or use the provided URL/SSH form).
3. If `siteDir` does not exist, clone the repository into `siteDir`.
4. Inside `siteDir`:
   - If it already contains a valid Docusaurus project (has `web/docusaurus.config.js` or `web/docusaurus.config.ts` and `web/package.json`), verify and continue.
   - Otherwise, execute: `npx --cache ./.npm-cache create-docusaurus@latest web classic -t`
5. Ensure directories `repos/` and `context/` exist at the workspace root.
6. Install GitHub Actions workflow: copy `specs/docusaurus_site/deploy-site.yml` to `<siteDir>/.github/workflows/deploy-site.yml` (create directories as needed).

## Verification
- Confirm that `<workspace>/<repo>/web/package.json` exists.
- Confirm presence of Docusaurus config file in `<workspace>/<repo>/web/`.
- Confirm `repos/` and `context/` directories are present at workspace root.
- Confirm workflow exists at `<workspace>/<repo>/.github/workflows/deploy-site.yml`.

## Example Output
```
Initializing Docusaurus site...
✓ Cloned repo into ./<repo>
✓ Initialized Docusaurus in ./<repo>/web
✓ Created ./repos and ./context folders
✓ Installed workflow at ./<repo>/.github/workflows/deploy-site.yml
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
