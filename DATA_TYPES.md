# Data types

These fields come from the plugin source. Query `describe_toolset` for the complete wire schema, including required fields and engine-reflected types.

## LandscapeInfo

Extent and calibration of the level's landscape. All lengths in cm.

| Field | Python declaration |
| --- | --- |
| `min_x` | `unreal.uproperty(float)` |
| `min_y` | `unreal.uproperty(float)` |
| `max_x` | `unreal.uproperty(float)` |
| `max_y` | `unreal.uproperty(float)` |
| `cell_size` | `unreal.uproperty(float, meta={'ToolTip': 'Distance between height samples.'})` |
| `base_z` | `unreal.uproperty(float, meta={'ToolTip': "World Z of a texel at the landscape's zero height."})` |
| `min_z` | `unreal.uproperty(float, meta={'ToolTip': 'Lowest world Z the heightmap can store.'})` |
| `max_z` | `unreal.uproperty(float, meta={'ToolTip': 'Highest world Z the heightmap can store.'})` |
| `proxy_count` | `unreal.uproperty(int, meta={'ToolTip': 'Loaded landscape proxies (World Partition cells).'})` |
| `material` | `unreal.uproperty(str)` |
| `paint_layers` | `unreal.uproperty(unreal.Array(str))` |

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

For Landscape height operations, `points` contains world-space vectors in centimetres and `kind` selects the relevant fields. Fields that do not apply to that kind are ignored. For paint operations, choose a material layer returned by `get_landscape_info`; the `shape` selects circle, path or polygon. A dry run in Asset Copy reports the same copy plan without performing the copy.

