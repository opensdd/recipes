---
name: docusaurus_plan
description: Prepare a plan to generate documentation into a Docusaurus site based on collected context.
---

Never modify site files. This command only prepares the execution plan.

## STRICT RULES
- NEVER edit or modify any files under the Docusaurus site during this command
- MUST save/overwrite `specs/docusaurus_site/plan.md`
- MUST read `specs/docusaurus_site/parameters.md` and consider the User Message if provided
- MUST return to earlier workflow steps if needed based on explorations

## Preconditions
1. Load Parameters: Read `specs/docusaurus_site/parameters.md`. It must contain:
   - Site Name (display title only)
   - GitHub Pages repository (org/repo) — required
   - A single "Context" field (free-form) that may include repositories, local files/folders, and remote URLs
   - Optional User Message with additional instructions
2. Inspect workspace folders `./context/` and `./repos/` to understand available materials.
3. If a previous plan exists at `specs/docusaurus_site/plan.md`, load it for continuity.
4. Assume the Docusaurus site will live in the web subfolder of the GitHub Pages repository.

## High-Level Workflow
1. Analyze Collected Inputs
   - Explore `./context/` files and any `./repos/` clones for content and domains
   - Identify key documentation areas (Overview, Getting Started, Tutorials, Guides, API, Configuration, FAQ)
   - Extract topics, APIs, and artifacts suitable for docs
2. Define Site Information Architecture
   - Propose sidebar structure (categories and pages)
   - Identify pages to generate under `web/docs/` and potential landing content under `web/src/pages/`
   - Note required assets (images, diagrams) if any
3. Map Inputs to Outputs
   - For each input source, define which docs/pages should reference or incorporate it
   - Plan any code snippet extraction from repos
   - Plan cross-references and navigation
4. GitHub Pages & Deployment Plan
   - Document configuration changes required in `docusaurus.config.*` for URL/baseUrl, organizationName, projectName
   - Plan npm scripts: at minimum `build` (using `docusaurus build`). Deployment will be handled by GitHub Actions.
   - Plan branch-based publishing to `gh-pages` via the provided GitHub Actions workflow
5. Persist the Plan
   - Save to `specs/docusaurus_site/plan.md`, overwriting previous version
6. Self-Revision Loop
   - Review for completeness and coherence
   - Iterate on analysis if gaps detected
7. Exit
   - Say: "Plan complete! The Docusaurus site generation plan has been saved to `specs/docusaurus_site/plan.md`."

## Failure Handling
- If `specs/docusaurus_site/parameters.md` is missing - notify user & exit
- If `./context/` or `./repos/` are missing - notify user that docusaurus_collect must be run first & exit

### Example Output Format
```markdown
# Plan - Docusaurus Site Generation

**Overview**
Collected inputs from 3 repositories, 5 local files, and 2 remote URLs. Goal: build a Docusaurus site named "MyDocs" hosted on GitHub Pages.

**Information Architecture**
- Sidebar
  - Getting Started
  - Guides
  - API Reference
  - Tutorials
  - FAQ

### Proposed Pages
- web/docs/intro.md — project overview
- web/docs/getting-started.md — installation and quickstart
- web/docs/guides/configuration.md — configuration reference
- web/docs/api/users.md — API endpoints and schemas
- web/docs/tutorials/first-steps.md — walkthrough tutorial

### Source Mapping
- repos/api-repo → web/docs/api/*.md (extracted from OpenAPI/specs)
- context/readme.md → web/docs/intro.md
- context/urls/guide.html → web/docs/guides/links.md (summarized links)

### Deployment
- organizationName: org
- projectName: repo
- url: https://org.github.io
- baseUrl: /repo/
- Ensure npm script exists: "build": "docusaurus build" (publishing is handled by the provided GitHub Actions workflow from `web/` build output)

```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
