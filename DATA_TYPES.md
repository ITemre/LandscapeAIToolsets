# Data types

These fields come from the plugin source (version 1.0.1). Query `describe_toolset` for the complete wire schema, including required fields and engine-reflected types.

## LandscapeInfo

Extent and calibration of the selected landscape. All lengths in cm.

| Field | Python declaration |
| --- | --- |
| `landscape` | `unreal.uproperty(str, meta={'ToolTip': 'Actor label of the Landscape the tools work on.'})` |
| `landscape_count` | `unreal.uproperty(int, meta={'ToolTip': 'Loaded Landscape actors in the level. Use select_landscape when more than one.'})` |
| `min_x` | `unreal.uproperty(float)` |
| `min_y` | `unreal.uproperty(float)` |
| `max_x` | `unreal.uproperty(float)` |
| `max_y` | `unreal.uproperty(float)` |
| `cell_size` | `unreal.uproperty(float, meta={'ToolTip': 'Distance between height samples.'})` |
| `base_z` | `unreal.uproperty(float, meta={'ToolTip': "World Z of a texel at the landscape's zero height."})` |
| `min_z` | `unreal.uproperty(float, meta={'ToolTip': 'Lowest world Z the heightmap can store.'})` |
| `max_z` | `unreal.uproperty(float, meta={'ToolTip': 'Highest world Z the heightmap can store.'})` |
| `proxy_count` | `unreal.uproperty(int, meta={'ToolTip': 'Loaded proxies of this landscape (World Partition cells).'})` |
| `material` | `unreal.uproperty(str)` |
| `paint_layers` | `unreal.uproperty(unreal.Array(str))` |
| `patch_layer` | `unreal.uproperty(str, meta={'ToolTip': 'Landscape Patch edit layer the edits go to. Empty until the first edit creates one.'})` |
| `edit_layers` | `unreal.uproperty(unreal.Array(str), meta={'ToolTip': 'All edit layers as "name (class)", in layer order.'})` |

## LandscapeCheck

Result of check_landscape: whether the level is ready for edits, and what to fix if not.

| Field | Python declaration |
| --- | --- |
| `ready` | `unreal.uproperty(bool, meta={'ToolTip': 'True when edits can run right away.'})` |
| `landscape` | `unreal.uproperty(str)` |
| `landscape_count` | `unreal.uproperty(int)` |
| `proxy_count` | `unreal.uproperty(int)` |
| `patch_layer` | `unreal.uproperty(str, meta={'ToolTip': 'Existing Landscape Patch edit layer, or empty when the first edit will create one.'})` |
| `paint_layers` | `unreal.uproperty(unreal.Array(str))` |
| `readback_ready` | `unreal.uproperty(bool, meta={'ToolTip': 'A height read at the landscape centre returned real data.'})` |
| `play_in_editor` | `unreal.uproperty(bool, meta={'ToolTip': 'True while Play-In-Editor runs; edits are refused then.'})` |
| `edits` | `unreal.uproperty(int, meta={'ToolTip': 'Patch actors of this toolset in the level, see list_edits.'})` |
| `issues` | `unreal.uproperty(unreal.Array(str), meta={'ToolTip': 'Blocking problems. Empty when ready.'})` |
| `notes` | `unreal.uproperty(unreal.Array(str), meta={'ToolTip': 'Non-blocking hints.'})` |

## EditInfo

One edit of this toolset that is loaded in the level: a height or paint patch actor.

| Field | Python declaration |
| --- | --- |
| `edit_id` | `unreal.uproperty(str, meta={'ToolTip': 'Pass to undo_edit.'})` |
| `kind` | `unreal.uproperty(str, meta={'ToolTip': 'height or paint.'})` |
| `actor` | `unreal.uproperty(str, meta={'ToolTip': 'Actor label in the outliner.'})` |
| `layer` | `unreal.uproperty(str, meta={'ToolTip': 'Edit layer the patch is bound to; "-Null-" means unbound.'})` |
| `priority` | `unreal.uproperty(float, meta={'ToolTip': 'Higher applies later (on top).'})` |
| `min_x` | `unreal.uproperty(float)` |
| `min_y` | `unreal.uproperty(float)` |
| `max_x` | `unreal.uproperty(float)` |
| `max_y` | `unreal.uproperty(float)` |

## TerrainOp

One height edit. `kind` selects which of the other fields are used:
flatten_polygon: points (x, y of the outline), height, falloff.
flatten_circle: points[0] centre, radius, height, falloff.
corridor: points (x, y, z centre line, z interpolated along it), half_width, falloff, crown.
raise: points[0] centre, radius, amount (cm added at the centre, dome falloff).
smooth: points[0] centre, radius, iterations.
noise: points[0] centre, radius, amount (+- cm), wavelength, seed.

| Field | Python declaration |
| --- | --- |
| `kind` | `unreal.uproperty(str)` |
| `points` | `unreal.uproperty(unreal.Array(unreal.Vector))` |
| `height` | `unreal.uproperty(float, meta={'ToolTip': 'Target world Z in cm.'})` |
| `radius` | `unreal.uproperty(float)` |
| `half_width` | `unreal.uproperty(float, meta={'ToolTip': 'Flat half width of a corridor.'})` |
| `falloff` | `unreal.uproperty(float, meta={'ToolTip': 'Width of the blend back to the existing terrain.'})` |
| `crown` | `unreal.uproperty(float, meta={'ToolTip': 'Extra cm at the corridor centre, for road camber.'})` |
| `amount` | `unreal.uproperty(float)` |
| `wavelength` | `unreal.uproperty(float)` |
| `seed` | `unreal.uproperty(int)` |
| `iterations` | `unreal.uproperty(int)` |

All numbers must be finite; non-finite values are rejected before anything is written.

## PaintOp

One paint stroke on a landscape paint layer. `shape` selects which fields are used:
circle: points[0] centre, radius, falloff.
path: points (x, y centre line), half_width, falloff.
polygon: points (x, y outline), falloff.

| Field | Python declaration |
| --- | --- |
| `layer` | `unreal.uproperty(str, meta={'ToolTip': 'Paint layer name from get_landscape_info.'})` |
| `shape` | `unreal.uproperty(str)` |
| `points` | `unreal.uproperty(unreal.Array(unreal.Vector))` |
| `radius` | `unreal.uproperty(float)` |
| `half_width` | `unreal.uproperty(float)` |
| `falloff` | `unreal.uproperty(float, meta={'ToolTip': 'Width of the soft edge, outside the shape.'})` |
| `strength` | `unreal.uproperty(float, meta={'ToolTip': '0..1 coverage at full weight. 0 is treated as 1.'})` |

## EditResult

What an edit changed.

| Field | Python declaration |
| --- | --- |
| `edit_id` | `unreal.uproperty(str, meta={'ToolTip': 'Pass to undo_edit. Empty when nothing changed.'})` |
| `cells_changed` | `unreal.uproperty(int)` |
| `min_x` | `unreal.uproperty(float)` |
| `min_y` | `unreal.uproperty(float)` |
| `max_x` | `unreal.uproperty(float)` |
| `max_y` | `unreal.uproperty(float)` |
| `max_raise` | `unreal.uproperty(float)` |
| `max_lower` | `unreal.uproperty(float)` |

## Reading structured arguments

For Landscape height operations, `points` contains world-space vectors in centimetres and `kind` selects the relevant fields. Fields that do not apply to that kind are ignored. For paint operations, choose a material layer returned by `get_landscape_info`; the `shape` selects circle, path or polygon. Call `check_landscape` before the first edit of a session and `list_edits` to find the ids of edits made earlier, for example before an editor restart.
