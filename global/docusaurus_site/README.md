# Docusaurus Site (GitHub Pages, branch-based)

This recipe scaffolds and fills a Docusaurus documentation website that is hosted via GitHub Pages using the gh-pages branch. It mirrors the docs_update flow (plan → generate) with additional steps for preparing the site and collecting context.

## What it does
- Prepare
  - Clones your pre-created GitHub repository that will host the site.
  - Initializes a Docusaurus project in the `web/` subfolder of that repository.
  - Creates workspace folders: `repos/` and `context/`.
  - Installs a GitHub Actions workflow at `.github/workflows/deploy-site.yml` to publish the built site to the `gh-pages` branch.
- Collect
  - Parses a single free-form Context field and collects inputs:
    - Clones GitHub repositories into `./repos/`
    - Copies local files/folders into `./context/`
    - Downloads remote URLs into `./context/urls/`
- Plan
  - Analyzes collected materials and drafts an information architecture and page plan.
  - Saves the plan to `specs/docusaurus_site/plan.md`.
- Generate
  - Creates/updates markdown pages under `<repo>/web/docs/` per the plan.
  - Updates sidebars and Docusaurus configuration for your GitHub Pages repo.
  - Ensures the GitHub Actions workflow is present at `<repo>/.github/workflows/deploy-site.yml`.

## Prerequisites
- A GitHub repository already created (required). Provide it as `org/repo` in the inputs.
- GitHub Pages will be served from the `gh-pages` branch (branch-based Pages). The included workflow publishes to `gh-pages` using `peaceiris/actions-gh-pages`.
- Node.js and npm available in the action runner (handled by the workflow). Locally, the agent may run `npx create-docusaurus@latest`.
- Network access for cloning and fetching remote resources.

## Inputs
- Site Name: Display title for the site (used in Docusaurus metadata; the site is scaffolded under `web/`).
- GitHub Pages Repository (required): The `org/repo` that will host the site. Must already exist.
- Context (optional): Free-form text containing GitHub repos, local file/folder paths, and remote URLs (one per line or any readable format). The agent parses and collects them.
- User Message (optional): Additional guidance for planning/generation.

## How to run
- Entry command: `docusaurus_run`
  - Executes: `/docusaurus_prepare` → `/docusaurus_collect` → `/docusaurus_plan` → `/docusaurus_generate`.
- After the run, commit and push the repository changes to your default branch (e.g., `main`). Pushing triggers the GitHub Action to publish the site to `gh-pages`.

## Outputs
- A Docusaurus site in the `web/` subfolder of your repository (`web/docusaurus.config.*`, `web/package.json`, `web/docs/`, `web/src/`, `web/static/`, etc.).
- Generated documentation pages under `<repo>/web/docs/` per the plan.
- Updated `web/sidebars` configuration for navigation.
- GitHub Actions workflow installed at `<repo>/.github/workflows/deploy-site.yml` that builds and deploys `web/` to `gh-pages`.
- Planning artifact at `specs/docusaurus_site/plan.md` in the workspace.

## Notes
- The recipe does not automatically push commits. You must push your changes to trigger the deploy workflow.
- For GitHub Pages settings, ensure your repository’s Pages is configured to serve from the `gh-pages` branch.
