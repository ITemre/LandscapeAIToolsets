# Changelog

## 1.0.1

- Patches bind to the selected Landscape and to its Landscape Patch edit layer explicitly. Any existing patch edit layer is used, whatever its name; when there is none, one named Patches is created. Before, edits failed on landscapes whose patch layer was not called Patches, or when a standard edit layer already used that name.
- Levels with several Landscapes: the tools measure and edit one Landscape at a time (`select_landscape`), instead of merging the extents of every landscape and reporting a misleading "readback not ready".
- New `check_landscape`: readiness check before editing (Landscape loaded, readback working, Play-In-Editor stopped, patch and paint layers, selected landscape), with the issues to fix.
- New `list_edits`: the height and paint edits still in the level, with extent and edit layer, so earlier edits can be undone after a restart.
- `get_landscape_info` reports the landscape label, the number of loaded landscapes, the patch edit layer and all edit layers.
- Clearer errors: a readback without data names the landscape and both likely causes; an unknown edit id points to `list_edits`.
- Height edits reject non-finite numbers instead of writing them into the patch.
- The bundled integration tests run on the loaded landscape (its centre and first paint layer) instead of a fixed test landscape.
- Patch priorities take foreign patch actors into account, so a new edit always lands on top.

## 1.0.0 release candidate

- Prepare a standalone Corivo Fab package from the 2026-10-18 backup.
- Add public documentation and explicit engine/plugin requirements.
- Keep native module names and MCP identifiers stable.

- Reject cold empty Landscape readbacks and complete real render synchronization.
- Verify height, painting, undo and save/reload on a fresh Landscape.
