# Astro Starlight Site (GitHub Pages, branch-based)

This recipe scaffolds and fills an Astro Starlight documentation website that is hosted via GitHub Pages using the gh-pages branch. It mirrors the docs_update flow (plan → generate) with additional steps for preparing the site and collecting context.

## What it does
- Prepare
  - Clones your pre-created GitHub repository that will host the site.
  - Initializes an Astro Starlight project in the `web/` subfolder of that repository.
  - Creates workspace folders: `repos/` and `context/`.
  - Installs a GitHub Actions workflow at `.github/workflows/deploy-site.yml` to publish the built site to the `gh-pages` branch.
- Collect
  - Parses a single free-form Context field and collects inputs:
    - Clones GitHub repositories into `./repos/`
    - Copies local files/folders into `./context/`
    - Downloads remote URLs into `./context/urls/`
- Plan
  - Analyzes collected materials and drafts an information architecture and page plan.
  - Saves the plan to `specs/astro_site/plan.md`.
- Generate
  - Creates/updates markdown pages under `<repo>/web/src/content/docs/` per the plan.
  - Updates Starlight configuration if needed for your GitHub Pages repo.
  - Ensures the GitHub Actions workflow is present at `<repo>/.github/workflows/deploy-site.yml`.

## Prerequisites
- A GitHub repository already created (required). Provide it as `org/repo` in the inputs.
- GitHub Pages will be served from the `gh-pages` branch (branch-based Pages). The included workflow publishes to `gh-pages` using `peaceiris/actions-gh-pages`.
- Node.js and npm available in the action runner (handled by the workflow). Locally, the agent may run `npm create astro@latest`.
- Network access for cloning and fetching remote resources.

## Inputs
- Site Name: Display title for the site (used in Starlight metadata; the site is scaffolded under `web/`).
- GitHub Pages Repository (required): The `org/repo` that will host the site. Must already exist.
- Context (optional): Free-form text containing GitHub repos, local file/folder paths, and remote URLs (one per line or any readable format). The agent parses and collects them.
- User Message (optional): Additional guidance for planning/generation.

## How to run
- Entry command: `astro_run`
  - Executes: `/astro_prepare` → `/astro_collect` → `/astro_plan` → `/astro_generate`.
- After the run, commit and push the repository changes to your default branch (e.g., `main`). Pushing triggers the GitHub Action to publish the site to `gh-pages`.

## Outputs
- An Astro Starlight site in the `web/` subfolder of your repository (`web/package.json`, `web/astro.config.*`, `web/starlight.config.*`, `web/src/content/docs/`, etc.).
- Generated documentation pages under `<repo>/web/src/content/docs/` per the plan.
- GitHub Actions workflow installed at `<repo>/.github/workflows/deploy-site.yml` that builds and deploys `web/` to `gh-pages`.
- Planning artifact at `specs/astro_site/plan.md` in the workspace.

## Notes
- The recipe does not automatically push commits. You must push your changes to trigger the deploy workflow.
- For GitHub Pages settings, ensure your repository’s Pages is configured to serve from the `gh-pages` branch.
