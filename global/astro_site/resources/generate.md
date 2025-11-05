---
name: astro_generate
description: Generate documentation content and configure the Astro Starlight site based on the prepared plan.
---

Purpose: Implement the site generation specified in the plan created by astro_plan.

## STRICT RULES
- MUST read and follow `specs/astro_site/plan.md` (created by astro_plan)
- MUST read `specs/astro_site/parameters.md` for Site Name and GitHub Pages repository
- MUST create and update documentation files inside `<workspace>/<repo>/web/src/content/docs/` and relevant site files
- MUST maintain documentation quality and consistency
- MUST ensure the GitHub Actions workflow from `specs/astro_site/deploy-site.yml` exists at `<workspace>/<repo>/.github/workflows/deploy-site.yml` (create directories as needed if missing)
- MUST NOT modify collected inputs under `./context/` or `./repos/`
- NEVER ask any questions or require interactive input

## Preconditions
1. Load Plan: Read `specs/astro_site/plan.md` containing:
   - Information architecture and proposed pages
   - Mapping from sources to outputs
   - Deployment configuration requirements
2. Load Parameters: Read `specs/astro_site/parameters.md` for:
   - Site Name (display title only)
   - GitHub Pages repository (org/repo) — required
   - Optional User Message/context
3. Verify that the Astro site directory exists and appears valid (created by astro_prepare).

## High-Level Workflow
1. Resolve Paths
   - Parse `<org>/<repo>` from the GitHub Pages repository value
   - `repoDir = <workspace>/<repo>` (the cloned GitHub Pages repository root)
   - `siteDir = <repoDir>/web` (the Astro site lives in the `web/` subfolder)
   - `docsDir = <siteDir>/src/content/docs`
2. Generate Docs per Plan
   - For each proposed page in the plan, create or overwrite corresponding markdown files under `web/src/content/docs/`
   - Extract or summarize content from `./context/` and `./repos/` as indicated by the plan
   - Ensure front-matter titles and slugs are set for navigation (Starlight supports frontmatter title, description)
3. Navigation / Sidebar
   - Update or create Starlight sidebar configuration if the template uses a sidebar config file; otherwise leverage content collections with file structure to determine nav
4. Configure for GitHub Pages (branch-based)
  - Use the provided GitHub Pages repository (org/repo):
    - In `astro.config.*` or `starlight.config.*`, set `site` (e.g., `https://org.github.io`) and `base` (e.g., `/repo/`) for project pages
    - Ensure npm scripts include `build` (e.g., `astro build`)
5. Install GitHub Actions Workflow
  - Create directory `<repoDir>/.github/workflows/` if it doesn't exist
  - Copy `specs/astro_site/deploy-site.yml` to `<repoDir>/.github/workflows/deploy-site.yml`
  - Do not rename or modify the workflow file contents
6. Assets
  - Copy any essential assets from `./context/` into appropriate locations under `siteDir/public/` or `siteDir/src/assets/` as referenced by pages
7. Verification
  - List created/updated files
  - Validate that `package.json` contains required scripts
  - Validate workflow exists at `<repoDir>/.github/workflows/deploy-site.yml`

## Failure Handling
- If `specs/astro_site/plan.md` is missing - notify that astro_plan must be run first & exit
- If parameters missing or siteDir invalid - notify & exit
- If file writes fail - report and continue where possible, then summarize issues

## Example Workflow Output
```
Loading plan...
✓ Loaded plan from specs/astro_site/plan.md

Generating documentation...
✓ Created web/src/content/docs/intro.md
✓ Created web/src/content/docs/getting-started.md
✓ Created web/src/content/docs/api/users.md

Configuring GitHub Pages...
✓ Set site and base in astro/starlight config
✓ Ensured npm script: build

Summary:
✓ Generated 7 documentation pages
✓ Updated site configuration
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
