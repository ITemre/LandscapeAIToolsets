# Runtime MCP schemas

Captured from `describe_toolset` in the native Unreal Engine 5.8.4 MCP server. These are documentation examples, not plugin source files. Refresh the schema from your own editor before automation.

## world_ground_toolset.toolsets.landscape_tools.LandscapeTools

```json
{
  "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools",
  "version": "1.0",
  "description": "Reads and sculpts the level's Landscape heightmap: flatten areas, cut road and alley\n    corridors, raise, smooth and roughen terrain. Each edit is a height patch actor on the\n    landscape's patch edit layer and can be undone by id.",
  "tools": [
    {
      "description": "Saves the landscape actors and ground paint patches that have unsaved changes.\n\n        Returns:\n            Package names that were saved.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.save_landscape",
      "inputSchema": {
        "type": "object"
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Reverts an earlier edit_heights or paint_layers call by deleting its patch actor. Later\n        edits in the same area stay as they are.\n\n        Args:\n            edit_id: EditResult.edit_id of the edit to revert.\n\n        Returns:\n            1 when the patch was removed. For a height edit from before heights were patches, the\n            number of cells restored (as a new patch over that region).",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.undo_edit",
      "inputSchema": {
        "type": "object",
        "properties": {
          "edit_id": {
            "type": "string"
          }
        },
        "required": [
          "edit_id"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "integer"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Paints landscape layers with a new weight patch actor (outliner folder GroundPaint).\n        The engine blends the strokes with the existing paint and keeps the weights normalized, so\n        to remove a layer from an area paint another layer over it, or undo the stroke.\n\n        Args:\n            ops: The strokes. Strokes of one layer are merged by maximum.\n\n        Returns:\n            The patch extent and the number of mask texels written.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.paint_layers",
      "inputSchema": {
        "type": "object",
        "properties": {
          "ops": {
            "type": "array",
            "items": {
              "title": "PaintOp",
              "type": "object",
              "properties": {
                "strength": {
                  "type": "number",
                  "description": "0..1 coverage at full weight. 0 is treated as 1."
                },
                "falloff": {
                  "type": "number",
                  "description": "Width of the soft edge, outside the shape."
                },
                "half_width": {
                  "type": "number"
                },
                "radius": {
                  "type": "number"
                },
                "points": {
                  "type": "array",
                  "items": {
                    "title": "Vector",
                    "type": "object",
                    "properties": {
                      "x": {
                        "type": "number"
                      },
                      "y": {
                        "type": "number"
                      },
                      "z": {
                        "type": "number"
                      }
                    },
                    "required": [
                      "x",
                      "y",
                      "z"
                    ]
                  }
                },
                "shape": {
                  "type": "string"
                },
                "layer": {
                  "type": "string",
                  "description": "Paint layer name from get_landscape_info."
                }
              },
              "required": [
                "strength",
                "falloff",
                "half_width",
                "radius",
                "points",
                "shape",
                "layer"
              ]
            }
          }
        },
        "required": [
          "ops"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "EditResult",
            "type": "object",
            "properties": {
              "max_lower": {
                "type": "number"
              },
              "max_raise": {
                "type": "number"
              },
              "max_y": {
                "type": "number"
              },
              "max_x": {
                "type": "number"
              },
              "min_y": {
                "type": "number"
              },
              "min_x": {
                "type": "number"
              },
              "cells_changed": {
                "type": "integer"
              },
              "edit_id": {
                "type": "string",
                "description": "Pass to undo_edit. Empty when nothing changed."
              }
            },
            "required": [
              "max_lower",
              "max_raise",
              "max_y",
              "max_x",
              "min_y",
              "min_x",
              "cells_changed",
              "edit_id"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Applies height edits in order on one shared region and lays the result over the landscape\n        as a new height patch actor (outliner folder GroundHeight). Batch related edits in one\n        call: each call exports the heightmap and adds one patch.\n\n        Args:\n            ops: The edits, applied in list order.\n\n        Returns:\n            The touched region and the largest raise and lowering in cm.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.edit_heights",
      "inputSchema": {
        "type": "object",
        "properties": {
          "ops": {
            "type": "array",
            "items": {
              "title": "TerrainOp",
              "type": "object",
              "properties": {
                "iterations": {
                  "type": "integer"
                },
                "seed": {
                  "type": "integer"
                },
                "wavelength": {
                  "type": "number"
                },
                "amount": {
                  "type": "number"
                },
                "crown": {
                  "type": "number",
                  "description": "Extra cm at the corridor centre, for road camber."
                },
                "falloff": {
                  "type": "number",
                  "description": "Width of the blend back to the existing terrain."
                },
                "half_width": {
                  "type": "number",
                  "description": "Flat half width of a corridor."
                },
                "radius": {
                  "type": "number"
                },
                "height": {
                  "type": "number",
                  "description": "Target world Z in cm."
                },
                "points": {
                  "type": "array",
                  "items": {
                    "title": "Vector",
                    "type": "object",
                    "properties": {
                      "x": {
                        "type": "number"
                      },
                      "y": {
                        "type": "number"
                      },
                      "z": {
                        "type": "number"
                      }
                    },
                    "required": [
                      "x",
                      "y",
                      "z"
                    ]
                  }
                },
                "kind": {
                  "type": "string"
                }
              },
              "required": [
                "iterations",
                "seed",
                "wavelength",
                "amount",
                "crown",
                "falloff",
                "half_width",
                "radius",
                "height",
                "points",
                "kind"
              ]
            }
          }
        },
        "required": [
          "ops"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "EditResult",
            "type": "object",
            "properties": {
              "max_lower": {
                "type": "number"
              },
              "max_raise": {
                "type": "number"
              },
              "max_y": {
                "type": "number"
              },
              "max_x": {
                "type": "number"
              },
              "min_y": {
                "type": "number"
              },
              "min_x": {
                "type": "number"
              },
              "cells_changed": {
                "type": "integer"
              },
              "edit_id": {
                "type": "string",
                "description": "Pass to undo_edit. Empty when nothing changed."
              }
            },
            "required": [
              "max_lower",
              "max_raise",
              "max_y",
              "max_x",
              "min_y",
              "min_x",
              "cells_changed",
              "edit_id"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns the landscape world Z in cm at each (x, y) position, bilinearly interpolated.\n        Positions off the loaded landscape return the landscape base Z.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.sample_heights",
      "inputSchema": {
        "type": "object",
        "properties": {
          "points": {
            "type": "array",
            "items": {
              "title": "Vector2D",
              "type": "object",
              "properties": {
                "x": {
                  "type": "number"
                },
                "y": {
                  "type": "number"
                }
              },
              "required": [
                "x",
                "y"
              ]
            }
          }
        },
        "required": [
          "points"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "type": "number"
            }
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns extent, height range, material and paint layers of the landscape.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.get_landscape_info",
      "inputSchema": {
        "type": "object"
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "LandscapeInfo",
            "type": "object",
            "properties": {
              "paint_layers": {
                "type": "array",
                "items": {
                  "type": "string"
                }
              },
              "material": {
                "type": "string"
              },
              "proxy_count": {
                "type": "integer",
                "description": "Loaded landscape proxies (World Partition cells)."
              },
              "max_z": {
                "type": "number",
                "description": "Highest world Z the heightmap can store."
              },
              "min_z": {
                "type": "number",
                "description": "Lowest world Z the heightmap can store."
              },
              "base_z": {
                "type": "number",
                "description": "World Z of a texel at the landscape's zero height."
              },
              "cell_size": {
                "type": "number",
                "description": "Distance between height samples."
              },
              "max_y": {
                "type": "number"
              },
              "max_x": {
                "type": "number"
              },
              "min_y": {
                "type": "number"
              },
              "min_x": {
                "type": "number"
              }
            },
            "required": [
              "paint_layers",
              "material",
              "proxy_count",
              "max_z",
              "min_z",
              "base_z",
              "cell_size",
              "max_y",
              "max_x",
              "min_y",
              "min_x"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    }
  ]
}
```

