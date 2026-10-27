# Landscape AI Toolsets: tool reference

Operation names and descriptions below are taken from the selected plugin source. Query `describe_toolset` for the exact runtime schema, required fields and defaults. C++ metadata can alter wire names; use the runtime schema. Python toolset names retain their module and class names.

## LandscapeTools

### `get_landscape_info`

```text

```

Returns extent, height range, material and paint layers of the landscape.

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
edits in the same area stay as they are.

Args:
    edit_id: EditResult.edit_id of the edit to revert.

Returns:
    1 when the patch was removed. For a height edit from before heights were patches, the
    number of cells restored (as a new patch over that region).

### `save_landscape`

```text

```

Saves the landscape actors and ground paint patches that have unsaved changes.

Returns:
    Package names that were saved.
