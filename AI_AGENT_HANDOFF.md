# Landscape AI Toolsets: agent guide

## Copy this setup prompt to your assistant

```text
Set up the purchased Landscape AI Toolsets plugin for my Unreal Engine 5.8 project.
Find the .uproject and ask me to select it if there are several. Verify the plugin
is already installed through Fab or in Project/Plugins/WorldGroundToolset.
Enable the plugin and its declared dependencies without replacing other settings.
Enable ModelContextProtocol, restart when necessary, and connect to the editor's
native Streamable HTTP MCP server on its configured loopback port.
Run list_toolsets and describe_toolset; verify these identifiers:
world_ground_toolset.toolsets.landscape_tools.LandscapeTools
Read https://github.com/ITemre/LandscapeAIToolsets/blob/main/README.md and the tool reference.
Start with read-only inspection in a disposable project and show me the first result.
```

## Working rules

- Inspect before changing content. Read `describe_toolset` rather than guessing arguments.
- Work on duplicates when changing meshes, bones, weight data or package references.
- Stop PIE before editor asset mutations; PIE probing is read-only unless explicitly documented.
- Check dirty levels before any workflow that opens or saves another level.
- Save only the intended assets. A tool success is not visual or gameplay acceptance.
- Keep asset paths and file paths distinct. `/Game/...` is a package path, not a Windows filename.
- Request operation-specific approval from your user for destructive changes or redistribution of assets.

## Product-specific constraints

- Requires a fully loaded, axis-aligned Landscape with edit layers and a Landscape Patch edit layer named Patches.
- Uses the first loaded Landscape actor. Work with one target Landscape in the level.
- The Landscape material must already expose the paint layers you want to use. No Landscape, material or paint-layer assets are included.
- The tool generates /Game/WorldGround/BP_GroundPatch on first use. This asset is created in your project and must remain with the edited level.
- Large reads export the whole Landscape heightmap to a float render target. Batch related edits and verify memory use on your own landscape.
- One patch actor is added per edit. undo_edit removes that operation; later patches remain in place.
- The supplied historical integration tests assume a particular landscape and GrassDry layer. Use the documented portable smoke checks in a fresh test map.
