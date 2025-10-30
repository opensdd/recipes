---
name: docs_update_rewrite
description: Execute documentation updates based on the prepared plan.
---

**Purpose**: Implement the documentation updates specified in the plan created by docs_update_plan.

## STRICT RULES
- **MUST** read and follow `specs/docs_update/plan.md` (created by docs_update_plan command)
- **MUST** read `specs/docs_update/parameters.md` for repository paths and context
- **MUST** update documentation files according to the plan's checklist
- **MUST** maintain documentation quality and consistency
- **NEVER** modify code files in dependent repositories

## Preconditions

1. **Load Plan**: Read `specs/docs_update/plan.md` (created by docs_update_plan) containing:
   - Overview of changes already analyzed
   - Summary of changes categorized by type
   - Detailed checklist of specific documentation updates to implement
   - Each checklist item specifies which docs to update and why

2. **Load Parameters**: Read `specs/docs_update/parameters.md` for:
   - Source repository locations (already analyzed during planning)
   - User message/context
   - Documentation repository path

## High-Level Workflow

1. **Load Plan and Context**
    * Read `specs/docs_update/plan.md` - this contains the complete analysis and checklist
    * Read `specs/docs_update/parameters.md` for repository paths
    * The plan was created by docs_update_plan and already contains:
        - Analysis of all changes in source repositories
        - Mapping to documentation areas
        - Specific updates needed for each documentation file

2. **Implement Documentation Updates**
    * Work through each checklist item in the plan sequentially
    * For each checklist item:
        1. Read the current documentation file specified in the checklist
        2. Reference source repositories for implementation details when needed:
           - Read specific changed files mentioned in the plan
           - Extract code examples, API signatures, configuration formats
           - Get exact parameter names, types, and descriptions
        3. Draft updated content that:
           - Implements the specific updates described in the plan
           - Maintains consistent style and tone with existing docs
           - Includes accurate code examples from source
           - Adds migration notes for breaking changes (as identified in plan)
           - Updates version information appropriately
        4. Edit documentation files with the new content
        5. Update the plan file to mark checklist item as complete (change `[ ]` to `[x]`)
        6. Ensure all cross-references are updated

3. **Verification**
    * Review all updated documentation:
        - Check for accuracy against source code
        - Verify code examples are correct and runnable
        - Ensure consistency across docs
        - Validate all links and references
        - Confirm all checklist items are marked complete in the plan
    * List all files modified

4. **Summary**
    * Provide detailed summary:
        - Number of checklist items completed
        - Documentation files updated (with file paths)
        - Key updates made for each file
        - Any issues or warnings encountered
        - Checklist items that couldn't be completed (if any)
    * Update the plan file with any final notes about the implementation

## Failure Handling

* If `specs/docs_update/plan.md` is missing - notify user that docs_update_plan must be run first & exit
* If `specs/docs_update/parameters.md` is missing - notify user & exit
* If source repositories are not found - notify user with paths expected
* If documentation files don't exist - create them or flag for review
* If a checklist item can't be completed - document the issue and continue with others

## Expected Plan Format (Input)

The plan is created by the docs_update_plan command and follows this format:

```markdown
# Plan - Documentation Updates

**Overview**
Analyzed changes across repo1, repo2 since v2.0. Found 15 significant changes requiring documentation updates across 4 documentation areas.

**Summary of Changes**
- New Features: Authentication API v2, webhook support
- Breaking Changes: Old auth endpoints deprecated
- API Modifications: New parameters for user endpoints
- Deprecations: Legacy configuration format

### Checklist
- [ ] Getting Started Guide - Update installation instructions
    - New dependency on library X (from repo1, commit abc123)
    - Changed configuration format (from repo2, see config.yml)
    - Add troubleshooting section for known issue Y
- [ ] API Reference - Update endpoint documentation
    - New `/api/v2/auth` endpoint (from repo1/src/auth/routes.ts)
    - Modified parameters for `/api/users` (added 'role' field)
    - Deprecate `/api/v1/auth` with migration guide
- [ ] Tutorials - Update code examples
    - Update "Quick Start" tutorial to use new auth flow
    - Add new tutorial for webhook integration
```

### parameters.md format (Example, it is free form):
```markdown
# Documentation Update Parameters

## User Message
Update docs to reflect the new authentication API released in v2.0

## Documentation Repository
- **Path**: ./docs-repo (or current working directory)

## Source Repositories
- **Repo**: https://github.com/org/api-repo.git
  - **Local Path**: ./repos/api-repo
  - **Branch**: main

- **Repo**: https://github.com/org/sdk-repo.git
  - **Local Path**: ./repos/sdk-repo
  - **Branch**: develop
```

## Example Workflow

```
Loading documentation update plan...
✓ Loaded plan from specs/docs_update/plan.md
✓ Plan contains 8 checklist items across 4 documentation areas

Loading parameters...
✓ Found 2 source repositories at specified paths
✓ Documentation repository: ./docs-repo

Processing checklist items...

[1/8] Updating Getting Started Guide - installation instructions
✓ Read docs/getting-started.md
✓ Referenced repos/api-repo/package.json for new dependency
✓ Updated installation section with library X
✓ Marked checklist item complete

[2/8] Updating Getting Started Guide - configuration format
✓ Read repos/sdk-repo/config.yml for new format
✓ Updated configuration examples
✓ Marked checklist item complete

[3/8] Updating API Reference - new authentication endpoint
✓ Read docs/api-reference.md
✓ Referenced repos/api-repo/src/auth/routes.ts for endpoint details
✓ Added `/api/v2/auth` documentation with examples
✓ Marked checklist item complete

[4/8] Updating API Reference - deprecation notice
✓ Added deprecation warning for `/api/v1/auth`
✓ Added migration guide section
✓ Marked checklist item complete

... (continuing with remaining items)

Verification...
✓ All code examples validated
✓ All cross-references updated
✓ 8/8 checklist items completed

Summary:
✓ Completed 8 checklist items
✓ Updated 4 documentation files:
  - docs/getting-started.md (installation, configuration)
  - docs/api-reference.md (new endpoints, deprecations)
  - docs/tutorials/quick-start.md (auth flow)
  - docs/tutorials/webhooks.md (new tutorial created)
✓ All updates reflect changes from v2.0 release
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
