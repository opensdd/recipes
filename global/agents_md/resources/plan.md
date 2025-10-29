---
name: agents_md_plan
description: Prepare a plan for generating agents.md files for the current repository.
---

**Never modify code.** This command designed to only prepare the execution plan.

## STRICT RULES
- **NEVER** edit or modify any code files
- **MUST** save/overwrite `specs/agents_md/plan.md`
- **MUST** return to earlier workflow steps if needed based on explorations


## Preconditions

1. **Load Goals**: Read `specs/agents_md/goal.md`. It describes how agent.md should look like.
2. If `plan.md` exists in a Task Folder, load it for continuity (enables incremental planning).

## High-Level Workflow

1. **Research The Areas**
    * Research the code to identify the high level areas that require AGENTS.MD file added for them.

    1. **Quick Scan** (Breadth-first):
       - Use Glob to identify candidate files by patterns
       - Use Grep to find relevant code sections by keywords
       - Use Explore agent to understand high-level structure
       - Build initial file/component map
    2. **Parallel Investigation**:
       - Launch multiple Explore agents concurrently when questions are independent
       - Each agent focuses on one question/domain
       - Aggregate findings when agents complete
       - Agents should serve a goal of high level understanding of the codebase to determine main areas to cover.
    

2. **Draft Plan**
    * Create a markdown plan (see *Example Output*). Include:
        * **Overview** - summary of the task/goal.
        * **Checklist** - specific areas to cover in the AGENTS.MD.

3. **Persist the plan**
    * Save draft to `specs/agents_md/plan.md`, overwriting previous version.

4. **Self Revision Loop**
    * Review the plan and make sure all the areas are covered, and the plan aligns with the guidelines for agents.md.
    * **MUST** return to Research (step 1) if any of the above is not met.
    * **MUST** continue this loop until all requirements are addressed with the plan with high confidence
    * Avoid duplicates, allow cross-reference where needed.
    * Make sure that the areas are large enough and not just every folder. Rule of thumb - one file per a subfolder of 50+ files.

5. **Exit**
    * Say: "Plan complete! The implementation plan has been saved to `<file location>`."

## Failure Handling

* If prerequisite files are missing - notify user & exit.

### Example Output Format

```markdown
# Plan - generating AGENT.MD files

**Overview**  
<One-paragraph goal statement>

### Checklist
- [ ] Area 1 - src/web. Web app root. Cover
    - testing strategies
    - build commands
    - core technologies used
    - notable pattenrs
- [ ] Area 2 - src/web/components. Root for re-usable components. Cover:
    - layout recommendations
    - main patterns (one exported component per file, use Tailwind for styling, ...)
- [ ] Area 3 - ...

```

---

Do not be conversational and don't ask user questions or confirmations. 
The flow is supposed to be fully autonomous and non-interactive.