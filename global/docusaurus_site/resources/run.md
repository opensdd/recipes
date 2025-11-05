---
name: docusaurus_run
description: Execute the complete Docusaurus site generation workflow from prepare to generate.
---

Execute a complete autonomous workflow for generating a Docusaurus website by running the sequence: /docusaurus_prepare → /docusaurus_collect → /docusaurus_plan → /docusaurus_generate.

## STRICT RULES
- MUST operate autonomously without asking user questions during execution
- MUST run commands in strict sequence: /docusaurus_prepare → /docusaurus_collect → /docusaurus_plan → /docusaurus_generate
- MUST continue until the "/docusaurus_generate" command is finished. Do not stop after intermediate steps, keep going till the end.
- MUST exit immediately if any command fails or reports errors

## High-Level Workflow

### Prepare Phase
- Execute: `/docusaurus_prepare` command
- Wait for command completion
- This initializes the Docusaurus site skeleton in the workspace
- On failure: Exit with error message
- On success: Move to next step

### Collect Phase
- Execute: `/docusaurus_collect` command
- Wait for command completion
- This gathers all inputs into ./repos and ./context
- On failure: Exit with error message
- On success: Move to next step

### Plan Phase
- Execute: `/docusaurus_plan` command
- Wait for command completion
- This analyzes inputs and creates a plan in specs/docusaurus_site/plan.md
- On failure: Exit with error message
- On success: Move to next step

### Generate Phase
- Execute: `/docusaurus_generate` command
- Wait for command completion
- This implements documentation and site configuration per the plan
- On failure: Exit with error message
- On success: Complete workflow and exit

---

Do not be conversational and don't ask user questions or confirmations. The flow is supposed to be fully autonomous and non-interactive.
