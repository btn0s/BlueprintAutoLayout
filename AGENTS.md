# AGENTS.md — BlueprintAutoLayout

This repository is the standalone Unreal Engine editor plugin root for **BlueprintAutoLayout**. Keep the repo plugin-only.

## Scope

Work only inside this repository's plugin files:

- `BlueprintAutoLayout.uplugin`
- `Source/BlueprintAutoLayout/`
- `README.md`
- `AGENTS.md`

Do not add host-project files such as `.uproject`, `Config/`, `Content/`, `Binaries/`, `Intermediate/`, `Saved/`, or root Unreal project metadata. This repo should remain installable by copying/cloning it into a project's `Plugins/BlueprintAutoLayout/` folder.

## Purpose

BlueprintAutoLayout adds one smart Blueprint graph context-menu command:

- **Organize Selected Nodes**

The intended behavior is a Blueprint-native layout:

- exec pins form the primary left-to-right spine;
- direct data/input dependencies are placed in the previous column;
- direct inputs stack gently in owner input-pin order;
- deeper input dependencies step farther left;
- branch/secondary exec paths use lower rows;
- generic layered layout remains the fallback when no K2 exec structure exists.

Prefer one smart mode with graceful fallbacks over adding multiple menu commands or configuration-heavy UI.

## Engineering Notes

- This is an editor-only C++ plugin.
- Avoid runtime modules and runtime dependencies.
- Avoid external layout libraries unless there is a strong reason; the current native algorithm is intentionally small and easy to tune.
- Keep undo support via scoped editor transactions.
- Keep context-menu integration through Unreal editor graph APIs rather than custom editor windows.
- Tune spacing constants near the top of `BlueprintAutoLayoutModule.cpp` when improving feel.

## Unreal Compatibility

The plugin was authored against Unreal Engine 5.7. If adapting to other engine versions:

- check GraphEditor and ToolMenus API changes;
- check `UEdGraphSchema_K2::PC_Exec` and K2 node includes;
- verify comment-node APIs before changing wrapping behavior.

## Validation

When validating inside a host Unreal project:

1. Copy or symlink this repository as `Plugins/BlueprintAutoLayout/`.
2. Enable the plugin in the host `.uproject`.
3. Build the host editor target.
4. Open a messy Blueprint graph.
5. Select nodes, right-click a selected node, and run **Organize Selected Nodes**.
6. Confirm undo works.

Do not commit generated build output from validation.

## Git Hygiene

Before committing, verify the tree contains only plugin-repo files. In particular, never push a host Unreal project into this repository.
