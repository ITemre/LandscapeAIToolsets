# Validation status

Practical checks recorded on 27 to 29 October 2026, using Unreal Engine 5.8.4 / Win64 in isolated local projects. Production projects and the original backup ZIPs were unchanged.

## Version 1.0.2 (29 October 2026)

- Package for the Fab technical review: source only. No Binaries, no LICENSE file, no test code, no levels or sample assets.
- Clean Win64 BuildPlugin with strict includes on the unpacked release ZIP, as Fab builds it: passed, no compiler warnings.
- Buyer install path: the plugin built from the ZIP was installed into a fresh Blueprint-only project; the editor mounted it, registered the toolset and answered `list_toolsets`, `describe_toolset` and `get_landscape_info` (expected error without a Landscape) over the native MCP server.
- The test suites used for earlier checks are internal and are no longer part of the package.

## Version 1.0.1 (28 October 2026)

- Clean Win64 BuildPlugin with strict includes: passed.
- Buyer install path: the release ZIP was unpacked into a fresh Blueprint-only project; the editor loaded the prebuilt binaries without recompiling, registered the toolset and answered `list_toolsets`, `describe_toolset` and tool calls over the native MCP server.
- Fresh landscape without any patch edit layer: the first edit created the layer `Patches`, the 300 cm raise was confirmed by an independent native read of the merged heightmap, painting set the layer weight to 255, undo restored both.
- Landscape with a standard (non-patch) edit layer named `Patches`: the edit created the patch layer `Patches1` and bound to it. Version 1.0.0 failed here.
- Landscape whose patch edit layer is named `MyPatches`: the edit bound to that layer. Version 1.0.0 failed here.
- Two landscapes in one level: `check_landscape` reports both, an edit outside the selected landscape is rejected with the landscape's extent, and after `select_landscape` the edit changed only the chosen landscape (confirmed by native reads of both). Version 1.0.0 merged both extents and reported a misleading "readback not ready".
- All 18 integration tests passed (internal suite, not shipped since 1.0.2) on the loaded landscape, with no skipped tests.
- Full practical workflow repeated on the rebuilt plugin: raise, patch ordering, undo of both patches, paint and paint undo, save, reload (239.84375 cm and weight 255 preserved) and individual undo after reopening.
- Runtime schema with nine tools captured from the native MCP server.

## Version 1.0.0 (27 October 2026)

- Fresh editor loading, native MCP tool discovery and schema reads: passed.
- All 15 existing Landscape integration tests passed on a fresh 127 x 127 sample Landscape, with no skipped tests.
- A 300 cm raise was confirmed both by the public tool and by independent native reads of the merged heightmap.
- Removing an older patch preserved the newer patch; removing both restored the baseline.
- Painting changed the actual GrassDry weight from 0 to 255; undo restored it to 0.
- Save/reload preserved a 239.84375 cm height and weight 255; the saved edits remained individually undoable after reopening.
- Empty readbacks during cold loading are rejected with a retry message before a height edit can use them. The release also includes an actual native render flush instead of the former ineffective console command.
- The painted and raised Landscape was rendered and the resulting image was inspected.

## Scope of acceptance

These are reproducible technical and visual smoke checks on the named fixtures. They do not claim exhaustive compatibility or performance at arbitrary project scale. Mac, Linux, other engine versions, large production Landscapes and World Partition levels with unloaded cells are outside the validated scope. Third-party example assets used locally for testing are not included in the product package.

## Fab status

Version 1.0.0 was submitted for review. Version 1.0.1 is prepared as an update; product approval, submission and publication are separate from these tests.
