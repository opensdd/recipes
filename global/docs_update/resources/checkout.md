---
name: docs_update_checkout
description: Clone and checkout git repositories for documentation update tasks.
---

**Purpose**: Clone git repositories specified in the parameters file for documentation update workflows.

## STRICT RULES
- **MUST** read `specs/docs_update/parameters.md` for repository list
- **MUST** clone each repository to appropriate location
- **MUST** checkout specified branches/commits if provided
- **MUST** verify successful checkout of all repositories
- **NEVER** modify repository contents during checkout

## Preconditions

1. **Load Parameters**: Read `specs/docs_update/parameters.md` to get:
   - List of repositories to clone
   - Target directories for each repository
   - Branch/tag/commit information (if specified)
   - Any additional checkout requirements

## High-Level Workflow

1. **Read Parameters File**
    * Load `specs/docs_update/parameters.md`
    * Parse repository information including:
        - Repository URLs (HTTPS or SSH)
        - Target directory paths
        - Branch names, tags, or commit SHAs
        - Any repository-specific instructions

2. **Prepare Workspace**
    * Verify parent directories exist
    * Create necessary directory structure
    * Check for existing repositories (avoid conflicts)

3. **Clone Repositories**
    * For each repository in the parameters file:
        1. Clone the repository to the specified target directory
        2. If branch/tag/commit is specified, checkout that reference
        3. Verify the checkout was successful
        4. Log the repository status

4. **Verification**
    * Confirm all repositories were cloned successfully
    * Verify correct branches/commits are checked out
    * Report any failures or issues

5. **Summary**
    * Provide a summary of:
        - Number of repositories cloned
        - List of successfully cloned repositories
        - Any failures or warnings
        - Directory locations

## Failure Handling

* If `specs/docs_update/parameters.md` is missing - notify user & exit
* If a repository clone fails - log error and continue with remaining repositories
* If a branch/commit doesn't exist - notify user with details
* Provide clear error messages for any git failures

## Expected Parameters File Format

The `specs/docs_update/parameters.md` file should contain repository information in a free form format.

## Example Output

```
Cloning repositories for documentation update...

✓ Cloned: https://github.com/org/repo-name.git → ./repos/repo-name (branch: main)
✓ Cloned: https://github.com/org/another-repo.git → ./repos/another-repo (branch: develop)

Summary: Successfully cloned 2/2 repositories
```

---

Do not be conversational and don't ask user questions or confirmations.
The flow is supposed to be fully autonomous and non-interactive.
