---
name: astro_run
description: Execute the complete Astro Starlight site generation workflow from prepare to generate.
---

Execute a complete autonomous workflow for generating an Astro Starlight website by running the sequence: /astro_prepare → /astro_collect → /astro_plan → /astro_generate.

## STRICT RULES
- MUST operate autonomously without asking user questions during execution
- MUST run commands in strict sequence: /astro_prepare → /astro_collect → /astro_plan → /astro_generate
- MUST continue until the "/astro_generate" command is finished. Do not stop after intermediate steps, keep going till the end.
- MUST exit immediately if any command fails or reports errors

## High-Level Workflow

### Prepare Phase
- Execute: `/astro_prepare` command
- Wait for command completion
- This initializes the Astro Starlight site skeleton in the workspace
- On failure: Exit with error message
- On success: Move to next step

### Collect Phase
- Execute: `/astro_collect` command
- Wait for command completion
- This gathers all inputs into ./repos and ./context
- On failure: Exit with error message
- On success: Move to next step

### Plan Phase
- Execute: `/astro_plan` command
- Wait for command completion
- This analyzes inputs and creates a plan in specs/astro_site/plan.md
- On failure: Exit with error message
- On success: Move to next step

### Generate Phase
- Execute: `/astro_generate` command
- Wait for command completion
- This implements documentation and site configuration per the plan
- On failure: Exit with error message
- On success: Complete workflow and exit

---

Do not be conversational and don't ask user questions or confirmations. The flow is supposed to be fully autonomous and non-interactive.
