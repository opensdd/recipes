---
name: astro_plan
description: Prepare a plan to generate documentation into an Astro Starlight site based on collected context.
---

Never modify site files. This command only prepares the execution plan.

## STRICT RULES
- NEVER edit or modify any files under the Astro site during this command
- MUST save/overwrite `specs/astro_site/plan.md`
- MUST read `specs/astro_site/parameters.md` and consider the User Message if provided
- MUST return to earlier workflow steps if needed based on explorations

## Preconditions
1. Load Parameters: Read `specs/astro_site/parameters.md`. It must contain:
   - Site Name (display title only)
   - GitHub Pages repository (org/repo) — required
   - A single "Context" field (free-form) that may include repositories, local files/folders, and remote URLs
   - Optional User Message with additional instructions
2. Inspect workspace folders `./context/` and `./repos/` to understand available materials.
3. If a previous plan exists at `specs/astro_site/plan.md`, load it for continuity.
4. Assume the Starlight docs will live in the web/src/content/docs subfolder of the GitHub Pages repository.

## High-Level Workflow
1. Analyze Collected Inputs
   - Explore `./context/` files and any `./repos/` clones for content and domains
   - Identify key documentation areas (Overview, Getting Started, Tutorials, Guides, API, Configuration, FAQ)
   - Extract topics, APIs, and artifacts suitable for docs
2. Define Site Information Architecture
   - Propose sidebar/collections structure (categories and pages)
   - Identify pages to generate under `web/src/content/docs/` and potential landing content
   - Note required assets (images, diagrams) if any
3. Map Inputs to Outputs
   - For each input source, define which docs/pages should reference or incorporate it
   - Plan any code snippet extraction from repos
   - Plan cross-references and navigation
4. GitHub Pages & Deployment Plan
   - Document configuration considerations for Astro/Starlight with GitHub Pages project pages
   - Plan npm scripts: at minimum `build` (using `astro build`). Deployment will be handled by GitHub Actions.
   - Plan branch-based publishing to `gh-pages` via the provided GitHub Actions workflow
5. Persist the Plan
   - Save to `specs/astro_site/plan.md`, overwriting previous version
6. Self-Revision Loop
   - Review for completeness and coherence
   - Iterate on analysis if gaps detected
7. Exit
   - Say: "Plan complete! The Astro site generation plan has been saved to `specs/astro_site/plan.md`."

## Failure Handling
- If `specs/astro_site/parameters.md` is missing - notify user & exit
- If `./context/` or `./repos/` are missing - notify user that astro_collect must be run first & exit

### Example Output Format
```markdown
# Plan - Astro Starlight Site Generation

**Overview**
Collected inputs from 3 repositories, 5 local files, and 2 remote URLs. Goal: build a Starlight site named "MyDocs" hosted on GitHub Pages.

**Information Architecture**
- Navigation
  - Getting Started
  - Guides
  - API Reference
  - Tutorials
  - FAQ

### Proposed Pages
- web/src/content/docs/intro.md — project overview
- web/src/content/docs/getting-started.md — installation and quickstart
- web/src/content/docs/guides/configuration.md — configuration reference
- web/src/content/docs/api/users.md — API endpoints and schemas
- web/src/content/docs/tutorials/first-steps.md — walkthrough tutorial

### Source Mapping
- repos/api-repo → web/src/content/docs/api/*.md (extracted from OpenAPI/specs)
- context/readme.md → web/src/content/docs/intro.md
- context/urls/guide.html → web/src/content/docs/guides/links.md (summarized links)

### Deployment
- Ensure npm script exists: "build": "astro build" (publishing is handled by the provided GitHub Actions workflow from `web/` build output to `gh-pages`)

```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
