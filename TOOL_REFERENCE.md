# Landscape AI Toolsets: tool reference

Operation names and descriptions below are taken from the plugin source (version 1.0.1). Query `describe_toolset` for the exact runtime schema, required fields and defaults. C++ metadata can alter wire names; use the runtime schema. Python toolset names retain their module and class names.

## LandscapeTools

Reads and sculpts the level's Landscape heightmap: flatten areas, cut road and alley corridors, raise, smooth and roughen terrain, and paint material layers. Each edit is a patch actor on the landscape's Landscape Patch edit layer and can be undone by id. Start with `check_landscape`; with several landscapes in the level choose one with `select_landscape`.

### `check_landscape`

```text

```

Checks whether the open level is ready for landscape edits, without changing anything: a Landscape is loaded, its height readback works, Play-In-Editor is not running, and which patch edit layer and paint layers exist. Call it first and fix the listed issues.

Returns:
    ready plus the blocking issues and hints (`LandscapeCheck`).

### `select_landscape`

```text
label: str
```

Chooses which Landscape the tools work on when the level holds several.

Args:
    label: Actor label of a loaded Landscape, as shown in the outliner. Empty selects the first loaded Landscape again.

Returns:
    The selected landscape, like get_landscape_info.

### `get_landscape_info`

```text

```

Returns extent, height range, material, paint layers and edit layers of the landscape.

### `list_edits`

```text

```

Lists the edits of this toolset that are loaded in the level, oldest first, so earlier edits can be undone after a restart.

Returns:
    One entry per height or paint patch actor (`EditInfo`).

### `sample_heights`

```text
points: list[unreal.Vector2D]
```

Returns the landscape world Z in cm at each (x, y) position, bilinearly interpolated.
Positions off the loaded landscape return the landscape base Z.

### `edit_heights`

```text
ops: list[TerrainOp]
```

Applies height edits in order on one shared region and lays the result over the landscape
as a new height patch actor (outliner folder GroundHeight). Batch related edits in one
call: each call exports the heightmap and adds one patch.

Args:
    ops: The edits, applied in list order.

Returns:
    The touched region and the largest raise and lowering in cm.

### `paint_layers`

```text
ops: list[PaintOp]
```

Paints landscape layers with a new weight patch actor (outliner folder GroundPaint).
The engine blends the strokes with the existing paint and keeps the weights normalized, so
to remove a layer from an area paint another layer over it, or undo the stroke.

Args:
    ops: The strokes. Strokes of one layer are merged by maximum.

Returns:
    The patch extent and the number of mask texels written.

### `undo_edit`

```text
edit_id: str
```

Reverts an earlier edit_heights or paint_layers call by deleting its patch actor. Later
edits in the same area stay as they are. list_edits shows the ids that are loaded.

Args:
    edit_id: EditResult.edit_id of the edit to revert.

Returns:
    1 when the patch was removed. For a height edit from before heights were patches, the
    number of cells restored (as a new patch over that region).

### `save_landscape`

```text

```

Saves the landscape actors and ground patches that have unsaved changes.

Returns:
    Package names that were saved.
