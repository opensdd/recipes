---
name: docs_update_plan
description: Prepare a plan for updating documentation based on changes in source repositories.
---

**Never modify code.** This command is designed to only prepare the execution plan.

## STRICT RULES
- **NEVER** edit or modify any code or documentation files
- **MUST** save/overwrite `specs/docs_update/plan.md`
- **MUST** return to earlier workflow steps if needed based on explorations


## Preconditions

1. **Load Parameters**: Read `specs/docs_update/parameters.md`. It must contain:
   - Name/path of the documentation repository
   - List of source repositories to analyze
   - Optional: time range for changes to analyze (e.g., last release, last N days)
   - Optional: user message with additional instructions or context
2. If `plan.md` exists in the Task Folder, load it for continuity (enables incremental planning).
3. **Consider User Message**: If parameters.md contains a user message, take it into account throughout the planning process (e.g., focus areas, priorities, specific concerns).

## High-Level Workflow

1. **Analyze Source Repository Changes**
    * For each source repository listed in parameters, analyze recent changes:

    1. **Repository Setup**:
       - Clone or locate the source repository
       - Determine the relevant time range (last release, recent commits, etc.)
       - Identify the main branch and relevant feature branches

    2. **Change Detection** (Parallel Investigation):
       - Launch multiple Explore agents concurrently for independent repositories
       - For each repository, identify:
         * New features added
         * Breaking changes or deprecations
         * API changes (new endpoints, modified signatures)
         * Configuration changes
         * Significant bug fixes that affect behavior
       - Use git log, git diff, and commit messages to understand changes
       - Use Grep to find relevant code sections that document public APIs
       - Use Explore agent to understand impact of changes

2. **Map Changes to Documentation**
    * Analyze the documentation repository structure:

    1. **Documentation Scan**:
       - Use Glob to identify documentation files by patterns (*.md, *.rst, etc.)
       - Build a map of documentation areas (getting started, API reference, tutorials, etc.)
       - Identify existing documentation that may be affected by source changes

    2. **Impact Analysis**:
       - Cross-reference source changes with documentation structure
       - Identify gaps where new features lack documentation
       - Flag outdated sections that reference deprecated functionality
       - Determine which guides/tutorials need updates

3. **Draft Plan**
    * Create a markdown plan (see *Example Output*). Include:
        * **Overview** - summary of repositories analyzed and scope of changes
        * **Summary of Changes** - high-level categorization of changes found
        * **Checklist** - specific documentation updates needed, organized by area

4. **Persist the Plan**
    * Save draft to `specs/docs_update/plan.md`, overwriting previous version.

5. **Self Revision Loop**
    * Review the plan and ensure:
      - All significant changes from source repositories are covered
      - Documentation areas are correctly identified
      - Updates are prioritized (critical breaking changes vs. minor enhancements)
    * **MUST** return to Analysis (step 1) if important changes were missed
    * **MUST** continue this loop until all significant changes are addressed with high confidence
    * Avoid duplicate updates, consolidate related changes where appropriate

6. **Exit**
    * Say: "Plan complete! The documentation update plan has been saved to `specs/docs_update/plan.md`."

## Failure Handling

* If `specs/docs_update/parameters.md` is missing - notify user & exit
* If source repositories cannot be accessed - notify user & exit
* If documentation repository cannot be accessed - notify user & exit

### Example Output Format

```markdown
# Plan - Documentation Updates

**Overview**
Analyzed changes across [repo1], [repo2], [repo3] since [date/version]. Found [N] significant changes requiring documentation updates across [M] documentation areas.

**Summary of Changes**
- New Features: [brief list]
- Breaking Changes: [brief list]
- API Modifications: [brief list]
- Deprecations: [brief list]

### Checklist
- [ ] Getting Started Guide - Update installation instructions
    - New dependency on library X (from repo1)
    - Changed configuration format (from repo2)
    - Add troubleshooting section for known issue Y
- [ ] API Reference - Update endpoint documentation
    - New `/api/v2/users` endpoint (from repo1)
    - Modified parameters for `/api/auth/login` (from repo2)
    - Deprecate `/api/v1/users` with migration guide
- [ ] Tutorials - Update code examples
    - Update "Quick Start" tutorial to reflect new authentication flow
    - Add new tutorial for feature X
    - Fix broken examples in "Advanced Usage" guide
- [ ] Configuration Reference - Update settings
    - Document new environment variables from repo3
    - Update default values that changed
    - Add deprecation warnings for old config format

```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.