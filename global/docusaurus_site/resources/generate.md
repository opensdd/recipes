---
name: docusaurus_generate
description: Generate documentation content and configure the Docusaurus site based on the prepared plan.
---

Purpose: Implement the site generation specified in the plan created by docusaurus_plan.

## STRICT RULES
- MUST read and follow `specs/docusaurus_site/plan.md` (created by docusaurus_plan)
- MUST read `specs/docusaurus_site/parameters.md` for Site Name and GitHub Pages repository
- MUST create and update documentation files inside `<workspace>/<repo>/web/docs/` and relevant site files
- MUST maintain documentation quality and consistency
- MUST configure Docusaurus for GitHub Pages deployment (branch-based) using the provided repository
- MUST ensure the GitHub Actions workflow from `specs/docusaurus_site/deploy-site.yml` exists at `<workspace>/<repo>/.github/workflows/deploy-site.yml` (create directories as needed if missing)
- MUST NOT modify collected inputs under `./context/` or `./repos/`
- NEVER ask any questions or require interactive input

## Preconditions
1. Load Plan: Read `specs/docusaurus_site/plan.md` containing:
   - Information architecture and proposed pages
   - Mapping from sources to outputs
   - Deployment configuration requirements
2. Load Parameters: Read `specs/docusaurus_site/parameters.md` for:
   - Site Name (display title only)
   - GitHub Pages repository (org/repo) — required
   - Optional User Message/context
3. Verify that the Docusaurus site directory exists and appears valid (created by docusaurus_prepare).

## High-Level Workflow
1. Resolve Paths
   - Parse `<org>/<repo>` from the GitHub Pages repository value
   - `repoDir = <workspace>/<repo>` (the cloned GitHub Pages repository root)
   - `siteDir = <repoDir>/web` (the Docusaurus site lives in the `web/` subfolder)
   - `docsDir = <siteDir>/docs`
2. Generate Docs per Plan
   - For each proposed page in the plan, create or overwrite corresponding markdown files under `web/docs/`
   - Extract or summarize content from `./context/` and `./repos/` as indicated by the plan
   - Ensure front-matter titles and slugs are set for navigation
3. Sidebars
   - Create or update `sidebars.js` or `sidebars.ts` to reflect the planned structure
4. Configure Docusaurus for GitHub Pages (branch-based)
  - Use the provided GitHub Pages repository (org/repo):
    - Set `organizationName`, `projectName` in `docusaurus.config.*`
    - Set `url` and `baseUrl` accordingly (url: `https://org.github.io`, baseUrl: `/repo/` for project pages)
    - Ensure `trailingSlash` is set as needed for your content
    - Add npm scripts to `package.json`: `build` (docusaurus build). The GitHub Action will publish to `gh-pages`. 
5. Install GitHub Actions Workflow
  - Create directory `<repoDir>/.github/workflows/` if it doesn't exist
  - Copy `specs/docusaurus_site/deploy-site.yml` to `<repoDir>/.github/workflows/deploy-site.yml`
  - Do not rename or modify the workflow file contents
6. Assets
  - Copy any essential assets from `./context/` into `siteDir/static/` as referenced by pages
7. Verification
  - List created/updated files
  - Validate sidebars config loads
  - Validate `package.json` contains required scripts
  - Validate workflow exists at `<repoDir>/.github/workflows/deploy-site.yml`

## Failure Handling
- If `specs/docusaurus_site/plan.md` is missing - notify that docusaurus_plan must be run first & exit
- If parameters missing or siteDir invalid - notify & exit
- If file writes fail - report and continue where possible, then summarize issues

## Example Workflow Output
```
Loading plan...
✓ Loaded plan from specs/docusaurus_site/plan.md

Generating documentation...
✓ Created web/docs/intro.md
✓ Created web/docs/getting-started.md
✓ Created web/docs/api/users.md
✓ Updated web/sidebars.js

Configuring GitHub Pages...
✓ Set organizationName=org, projectName=repo
✓ Updated url and baseUrl
✓ Ensured npm script: build

Summary:
✓ Generated 7 documentation pages
✓ Updated sidebars and site configuration
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
