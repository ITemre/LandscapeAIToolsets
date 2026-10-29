# Landscape AI Toolsets: agent guide

## Copy this setup prompt to your assistant

```text
Please set up the "Landscape AI Toolsets" Unreal Engine plugin for me and connect yourself to it.

1. Find my Unreal project (.uproject). Ask me to select it if there are several. It must use
   Unreal Engine 5.8 on Windows. Preserve unsaved editor work before editing settings or restarting.
2. Check that the purchased plugin is installed: either in the engine under
   Engine/Plugins/Marketplace or as <Project>/Plugins/WorldGroundToolset. If it is missing, stop and
   tell me to install it from Fab first.
3. Enable these plugins in the .uproject "Plugins" list (add entries with "Enabled": true and
   preserve all other settings): WorldGroundToolset, ToolsetRegistry, PythonScriptPlugin,
   LandscapePatch, EditorScriptingUtilities, ModelContextProtocol.
4. Start the editor's native MCP server with -ModelContextProtocolStartServer, or enable auto start
   in Project Settings > Plugins > Model Context Protocol and restart safely. Confirm the server's
   configured port. The default endpoint is http://127.0.0.1:8000/mcp. Verify it belongs to this project.
5. Add that endpoint to your own MCP configuration as an HTTP (Streamable HTTP) server named
   "unreal", then reload your MCP connection if needed. Preserve existing server entries; use a
   different name if "unreal" already points to another project. Keep the endpoint on loopback.
6. Call list_toolsets and confirm every identifier below is listed. Call describe_toolset for each:
   world_ground_toolset.toolsets.landscape_tools.LandscapeTools
7. Read the agent guide at
   https://github.com/ITemre/LandscapeAIToolsets/blob/main/AI_AGENT_HANDOFF.md
   (also shipped as Documentation/AI_AGENT_HANDOFF.md) and follow the tool reference and project rules.
8. Do not create or modify a Landscape during setup. If no Landscape is loaded, report it as a
   content prerequisite after verifying the MCP toolset.
   Tell me in two sentences which project and endpoint are connected, whether all required toolsets
   are available, and what I can ask you to do next.
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

- Requires a fully loaded, axis-aligned Landscape. The edits go to a Landscape Patch edit layer; any existing patch edit layer is used, whatever its name, and the first edit creates one named Patches when there is none.
- Works on one Landscape at a time: the first loaded one, or the one chosen with select_landscape when the level holds several.
- The Landscape material must already expose the paint layers you want to use. No Landscape, material or paint-layer assets are included.
- The tool generates /Game/WorldGround/BP_GroundPatch on first use. This asset is created in your project and must remain with the edited level.
- Large reads export the whole Landscape heightmap to a float render target. Batch related edits and verify memory use on your own landscape.
- One patch actor is added per edit. undo_edit removes that operation; later patches remain in place.
