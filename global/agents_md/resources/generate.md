---
name: agents_md_generate
description: Executes AGENTS.md files generation
---

Execute a complete autonomous workflow for generating AGENTS.md files according to a prepared plan.

## STRICT RULES
- **MUST** operate autonomously without asking user questions during execution

## Preconditions

1. **Load Goals**: Read `specs/agents_md/goal.md`. It describes how agent.md should look like.
2. **Load Plan**: `specs/agents_md/plan.md`. If plan doesn't exist, exit with error.

## High-Level Workflow

### Plan Alignment
Understand the goals and a plan.

### Investigate with Parallel Exploration

Use Task tool with `subagent_type=Explore` for efficient codebase navigation

For each area:

1. **Quick Scan** (Breadth-first):
    - Use Glob to identify candidate files by patterns
    - Use Grep to find relevant code sections by keywords
    - Use Explore agent to understand high-level structure
    - Build initial file/component map
2. **Parallel Investigation** (for independent questions):
    - Launch multiple Explore agents concurrently when questions are independent
    - Each agent focuses on one question/domain
    - Aggregate findings when agents complete
3. **Deep Dive** (Depth-first):
    - Read key files identified in quick scan
    - Trace code flows and dependencies
    - Understand implementation details
    - Document edge cases and complexities 

### Generate AGENTS.md

Based on the investigation, generate corresponding AGENTS.md files.

---

Do not be conversational and don't ask user questions or confirmations. The flow is supposed to be fully autonomous and non-interactive.