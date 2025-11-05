---
name: docusaurus_collect
description: Collect and normalize all input sources (repos, local files/folders, remote URLs) into workspace context/ and repos/.
---

Purpose: Prepare content for documentation generation by gathering all specified inputs.

## STRICT RULES
- MUST read `specs/docusaurus_site/parameters.md`
- MUST clone all provided GitHub repositories into `./repos/` (workspace root)
- MUST copy provided local files into `./context/`
- MUST recursively copy provided local folders into `./context/<folder-name>/`
- MUST download each provided remote URL into `./context/urls/` (use deterministic filenames)
- MUST NOT modify the contents of collected inputs
- MUST produce a summary of what was collected
- NEVER ask any questions or require interactive input

## Preconditions
1. Ensure that `./context/` and `./repos/` directories exist (create if missing).
2. Load parameters from `specs/docusaurus_site/parameters.md` and parse the single "Context" field (free-form text). From it, extract:
   - GitHub repositories (URLs or org/repo notations)
   - Local file paths
   - Local folder paths
   - Remote URLs (http/https)

## High-Level Workflow
1. Clone Repositories
   - For each repository URL, compute target path `./repos/<repo-name>`
   - If target exists and is a git repo, fetch + checkout default branch; else clone
   - Verify successful checkout
2. Copy Local Files
   - For each file path, validate existence then copy into `./context/`
3. Copy Local Folders
   - For each folder path, validate existence then copy recursively into `./context/`
4. Download Remote URLs
   - Create `./context/urls/`
   - For each URL, download to a deterministic filename, e.g., `<index>_<slug>.txt` or original filename if present
5. Verification & Summary
   - Confirm presence of collected artifacts
   - Output a summary table of counts and locations

## Example Output
```
Collecting inputs...
✓ Repos cloned: 3 (./repos/api, ./repos/web, ./repos/sdk)
✓ Files copied: 5 → ./context/
✓ Folders copied: 2 → ./context/
✓ URLs downloaded: 4 → ./context/urls/
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
