# Landscape AI Toolsets: troubleshooting

| Symptom | Check |
| --- | --- |
| Plugin is missing | Install the purchased plugin into the correct engine or project Plugins directory. |
| Module cannot load | Use UE 5.8 / Win64 and rebuild the source plugin with the supported toolchain. |
| Toolset is missing | Enable all declared plugins, restart, then inspect Python and plugin errors in the Output Log. |
| MCP cannot connect | Confirm the native server is running, its port, the client transport and loopback address. |
| An argument is rejected | Refresh `describe_toolset` and follow its runtime schema. |
| Assets or classes are missing | Supply the required project assets and enable the engine dependency that owns the class. |
| An operation succeeds but looks wrong | Inspect it in the editor and undo or restore your duplicate/backup before continuing. |
| `check_landscape` reports no readback data | Let the level finish loading, make sure the cells you edit are loaded, then call it again. |
| The wrong Landscape is edited | Several Landscapes are loaded. Call `select_landscape` with the actor label from the outliner. |
| `undo_edit` says there is no such edit | Call `list_edits`: the patch actor was deleted or renamed, or lies in a level that is not loaded. |
| The patch lands on an unexpected edit layer | The tools use the first Landscape Patch edit layer of the selected landscape (shown by `get_landscape_info`); create one named Patches before the first edit if you want that name. |

## Before filing an issue

Reproduce in a fresh test project, record the minimal operation sequence, and include the engine and plugin versions. Do not attach your entire paid project, credentials or third-party assets.

## First smoke check

Create a disposable Landscape and make sure the cells you want to edit are loaded. Call check_landscape and fix anything it lists, then get_landscape_info, sample a centre point, apply a small raise, sample again, undo_edit using the returned edit_id and sample once more. Paint an existing material layer and inspect the result before saving. list_edits shows every edit that is still in the level, also after a restart.
