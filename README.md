# OpenSDD Recipes

This repository hosts the catalog of official OpenSDD recipes. Each recipe packages the context files, permissions, and execution metadata needed for the OpenSDD CLI (`osdd`) and compatible IDE integrations.

Use this guide to explore the repository structure and to contribute new recipes.

---

## Repository Layout

```
global/
├── agents_md/
│   └── recipe.json          # Example recipe in JSON form for testing agents
└── docs_update/
    └── recipe.yaml          # Documentation update automation
```

- **Global Recipes** – recipes within `global` directory are available for execution by just ID. Those recipes are carefully reviewed before they are merged and are considered safe for execution.
- **Manifests** – Recipes are described using `recipe.yaml` (YAML and JSON are supported). The format serializes the `osdd.recipes.ExecutableRecipe` protobuf defined in [opensdd/osdd-api](https://github.com/opensdd/osdd-api).
- **Supporting assets** – Context files referenced by a manifest (for example prompts, templates, diagrams) reside alongside the manifest. Use subfolders when a recipe ships multiple assets.

---

## End-User Experience

End users install the [OpenSDD CLI](https://github.com/opensdd/osdd-cli#readme) and launch recipes by referencing the manifest path:

```bash
osdd recipe execute docs_update --ide claude
```

---

## Adding a New Recipe

1. **Plan the workflow**
   Identify the automation goal, required context files, and any commands that must run. Review existing manifests in `global/` for reference.

2. **Create a folder**
   Add a descriptive directory inside the appropriate collection, e.g. `global/my_new_flow/`.

3. **Author the manifest**
   Create `recipe.yaml` (preferred) or `recipe.json` that serializes an `ExecutableRecipe`. Key sections include:
   - `recipe.prefetch` for commands that gather data before execution.
   - `recipe.context` for files pulled into the workspace (refer to Git paths, inline text, or user prompts).
   - `recipe.ide` for UI commands and permission policies.
   - `entry_point` for IDE-specific launch settings (required `ide_type`, optional prompts/commands, workspace requirements).
   Use the field comments in [`opensdd/osdd-api`](https://github.com/opensdd/osdd-api/tree/main/protos/osdd) as a reference for each attribute.

4. **Add supporting files**
   Store templates, prompt text, or images next to the manifest. Reference them using `context` entries (Git, text, or local file sources).

5. **Test locally**
   Run the manifest with a local checkout of `osdd-cli`:
   ```bash
   osdd recipe execute dummy --recipe-file ./global/my_new_flow/recipe.yaml --ide claude
   ```
   The positional `dummy` ID is ignored when `--recipe-file` is provided.

6. **Document user input**
   Ensure every `UserInputParameter` includes an informative `description` and sets `optional` appropriately so the CLI can guide the user.

7. **Submit a pull request**
   Include a README snippet or comment explaining the intent of the recipe, required permissions, and expected outputs. Link to complementary documentation (CLI README, API docs) when relevant.

---

## Best Practices

- **Keep manifests declarative** – Delegate scripting or heavy logic to referenced context files or prefetch commands; keep the manifest readable.
- **Scope permissions tightly** – Use allow/deny lists in `recipe.ide.permissions` to restrict shell commands, file access, and network use.
- **Reuse shared assets** – When multiple recipes share prompts or scripts, place them in a common location and reference via `common.GitReference`.
- **Cross-link documentation** – Mention relevant sections from the CLI README and API docs so users can troubleshoot or extend the automation.
- **Keep recipes safe** - Most importantly, all recipes should be safe by a user to execute.

---

## Need Help?

- File issues in this repository for recipe-specific questions.
- Raise CLI usage questions in [opensdd/osdd-cli](https://github.com/opensdd/osdd-cli/issues).
- Discuss API or protobuf schema topics in [opensdd/osdd-api](https://github.com/opensdd/osdd-api/issues).

Happy automating!
