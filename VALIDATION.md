# Validation status

Practical checks recorded on 27 October 2026, using Unreal Engine 5.8.4 / Win64 in isolated local projects. Production projects and the original backup ZIPs were unchanged.

## Technical and practical checks

- Clean Win64 BuildPlugin with strict includes: passed.
- Fresh editor loading, native MCP tool discovery and schema reads: passed.
- All 15 existing Landscape integration tests passed on a fresh 127 x 127 sample Landscape, with no skipped tests.
- A 300 cm raise was confirmed both by the public tool and by independent native reads of the merged heightmap.
- Removing an older patch preserved the newer patch; removing both restored the baseline.
- Painting changed the actual GrassDry weight from 0 to 255; undo restored it to 0.
- Save/reload preserved a 239.84375 cm height and weight 255; the saved edits remained individually undoable after reopening.
- Empty readbacks during cold loading are rejected with a retry message before a height edit can use them. The release also includes an actual native render flush instead of the former ineffective console command.
- The painted and raised Landscape was rendered and the resulting image was inspected.

## Scope of acceptance

These are reproducible technical and visual smoke checks on the named fixtures. They do not claim exhaustive character compatibility or performance at arbitrary project scale. Mac, Linux, other engine versions, large production Landscapes and all possible third-party rigs are outside the validated scope. Third-party example assets used locally for testing are not included in the product package.

## Fab status

Prepared as a release candidate. Product approval, submission and publication are separate from these tests.
