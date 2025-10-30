---
name: docs_update_run
description: Execute complete documentation update workflow from checkout to implementation.
---

Execute a complete autonomous workflow for updating documentation by running the full sequence: /docs_update_checkout → /docs_update_plan → /docs_update_rewrite.

## STRICT RULES
- **MUST** operate autonomously without asking user questions during execution
- **MUST** run commands in strict sequence: /docs_update_checkout → /docs_update_plan → /docs_update_rewrite
- **MUST** continue until the "/docs_update_rewrite" command is finished. Do not stop after checkout or plan, keep going till the end.
- **MUST** exit immediately if any command fails or reports errors


## High-Level Workflow


### Checkout Phase Execution
- Execute: `/docs_update_checkout` command
- Wait for command completion
- This clones/verifies all necessary repositories (documentation and source repos)
- On failure: Exit with error message
- On success: Move to next step

### Plan Phase Execution
- Execute: `/docs_update_plan` command
- Wait for command completion
- This analyzes changes in source repositories and creates a plan in specs/docs_update/plan.md
- On failure: Exit with error message
- On success: Move to next step

### Rewrite Phase Execution
- Execute: `/docs_update_rewrite` command
- Wait for command to finish
- This implements all documentation updates specified in the plan
- On failure: Exit with error message
- On success: Complete workflow and exit

---

Do not be conversational and don't ask user questions or confirmations. The flow is supposed to be fully autonomous and non-interactive.