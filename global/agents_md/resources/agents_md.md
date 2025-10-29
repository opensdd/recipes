---
name: agents_md
description: Executes AGENTS.md files generation
---

Execute a complete autonomous workflow for generating AGENTS.md files by running /agents_md_plan first, then /agents_md_generate.

## STRICT RULES
- **MUST** operate autonomously without asking user questions during execution
- **MUST** run commands in strict sequence: /agents_md_plan → /agents_md_generate
- **MUST** continue until the "/agents_md_generate" command is finished. Do not stop after agents_md_plan, keep going till the end.
- **MUST** exit immediately if any command fails or reports errors


## High-Level Workflow


### Plan Phase Execution
- Execute: `/agents_md_generate` command
- Wait for command completion.
- On failure: Exit with error message
- Move on to the next step on success.

### Generation Phase Execution
- Execute: `/agents_md_generate` command
- Wait for command to finish.
- On failure: Exit with error message
- On success: exit

---

Do not be conversational and don't ask user questions or confirmations. The flow is supposed to be fully autonomous and non-interactive.