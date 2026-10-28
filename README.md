# Landscape AI Toolsets

Read, sculpt and paint Unreal Landscape terrain with patch actors through Unreal Editor's native MCP server.

Public documentation for the commercial Fab plugin from Corivo. This repository contains documentation only. The plugin source, binaries and project files are distributed separately through Fab.

<!-- AI_SETUP_PROMPT_START -->
## Set up with your AI assistant

Copy the whole block below into an AI assistant that can run commands, edit files on your computer,
and connect to MCP servers. It checks the installation, enables the required plugins and connects itself.

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

<!-- AI_SETUP_PROMPT_END -->

## Features

- Inspect Landscape bounds, height calibration, loaded proxies and paint layers.
- Sample exact world heights in centimetres.
- Flatten circles or polygons and shape road corridors, smoothing, raises and noise.
- Paint material layers using circle, path and polygon masks.
- Undo a tool operation by removing its patch actor, list the edits that are still in the level and save only relevant dirty packages.
- Check the level before editing: check_landscape reports readiness, patch and paint layers, and which Landscape is selected.

## Requirements

- Unreal Engine 5.8, Windows 64-bit editor. Other engines and platforms are not claimed.
- Engine plugins: **ToolsetRegistry**, **PythonScriptPlugin**, **LandscapePatch**, **EditorScriptingUtilities**, **ModelContextProtocol**.
- An HTTP MCP-compatible assistant or client. The client and any paid AI service are separate from this product.
- Editor only. The tools do not run in a packaged game.

## Installation and MCP connection

1. Install the product from Fab when available, or copy the purchased `WorldGroundToolset` plugin folder to `<Project>/Plugins/`. The package ships Win64 editor binaries for UE 5.8 and the source; a C++ toolchain is only needed if the editor asks to rebuild the plugin.
2. Enable `WorldGroundToolset` and its dependencies under **Edit > Plugins**, plus **Model Context Protocol**. Restart the editor.
3. Start the editor's native MCP server with `-ModelContextProtocolStartServer`, or enable auto start under **Project Settings > Plugins > Model Context Protocol**.
4. Connect your client using Streamable HTTP. The default endpoint is `http://127.0.0.1:8000/mcp`; confirm the port in the editor settings. Keep it on loopback.
5. Call `list_toolsets`, then `describe_toolset` for the identifiers listed below. Use the returned input schemas for your installed engine.

```json
{"mcpServers":{"unreal":{"type":"http","url":"http://127.0.0.1:8000/mcp"}}}
```

The JSON example is a common client configuration, not a universal format. Follow your client's setup instructions.

## Toolset identifiers

- `world_ground_toolset.toolsets.landscape_tools.LandscapeTools`

See the [captured runtime schemas](RUNTIME_SCHEMAS.md) for the verified wire-format arguments.

## First workflow

Create a disposable Landscape and make sure the cells you want to edit are loaded. Call check_landscape and fix anything it lists, then get_landscape_info, sample a centre point, apply a small raise, sample again, undo_edit using the returned edit_id and sample once more. Paint an existing material layer and inspect the result before saving. list_edits shows every edit that is still in the level, also after a restart.

The [data types](DATA_TYPES.md) describe structured arguments. The [agent guide](AI_AGENT_HANDOFF.md) includes a setup prompt and working rules. The [tool reference](TOOL_REFERENCE.md) documents every exposed operation. Read [troubleshooting](TROUBLESHOOTING.md) before using the tools on production content.

## Limitations

- Requires a fully loaded, axis-aligned Landscape. The edits go to a Landscape Patch edit layer; any existing patch edit layer is used, whatever its name, and the first edit creates one named Patches when there is none.
- Works on one Landscape at a time: the first loaded one, or the one chosen with select_landscape when the level holds several.
- The Landscape material must already expose the paint layers you want to use. No Landscape, material or paint-layer assets are included.
- The tool generates /Game/WorldGround/BP_GroundPatch on first use. This asset is created in your project and must remain with the edited level.
- Large reads export the whole Landscape heightmap to a float render target. Batch related edits and verify memory use on your own landscape.
- One patch actor is added per edit. undo_edit removes that operation; later patches remain in place.
- The bundled integration tests run on whatever Landscape is loaded (centre of the landscape, first paint layer of its material) and undo their edits. Run them in a disposable test map, not on production content.


Cold loading: if a height read reports that the readback returned no data, let the level finish loading and retry, or check that the region lies on the loaded part of the selected landscape. check_landscape shows both. The tool refuses to use an empty readback for editing.

## Validation and release availability

This documentation accompanies release preparation. It is not a claim that Fab has approved this product or that visual, gameplay or performance acceptance is complete. See [VALIDATION.md](VALIDATION.md) for the latest recorded technical checks. Until a Fab product link is added here, this product has no confirmed public purchase link.

## Support

Report reproducible problems through [GitHub issues](https://github.com/ITemre/LandscapeAIToolsets/issues). Include the engine version, plugin version, tool name, minimal arguments and error text. Remove credentials and private project data from reports.

## Licence

The commercial plugin is intended for sale under the [Fab Standard License](https://www.fab.com/eula), with Personal and Professional price tiers. Public documentation does not make the plugin open source or grant rights to redistribute it. Copyright 2026 Corivo. All rights reserved.
