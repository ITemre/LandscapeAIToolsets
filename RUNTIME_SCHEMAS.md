# Runtime MCP schemas

Captured from `describe_toolset` in the native Unreal Engine 5.8.4 MCP server (plugin version 1.0.1). These are documentation examples, not plugin source files. Refresh the schema from your own editor before automation.

## world_ground_toolset.toolsets.landscape_tools.LandscapeTools

```json
{
  "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools",
  "version": "1.0",
  "description": "Reads and sculpts the level's Landscape heightmap: flatten areas, cut road and alley\n    corridors, raise, smooth and roughen terrain, and paint material layers. Each edit is a patch\n    actor on the landscape's Landscape Patch edit layer and can be undone by id. Start with\n    check_landscape; with several landscapes in the level choose one with select_landscape.",
  "tools": [
    {
      "description": "Saves the landscape actors and ground patches that have unsaved changes.\n\n        Returns:\n            Package names that were saved.",
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
      "description": "Reverts an earlier edit_heights or paint_layers call by deleting its patch actor. Later\n        edits in the same area stay as they are. list_edits shows the ids that are loaded.\n\n        Args:\n            edit_id: EditResult.edit_id of the edit to revert.\n\n        Returns:\n            1 when the patch was removed. For a height edit from before heights were patches, the\n            number of cells restored (as a new patch over that region).",
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
      "description": "Lists the edits of this toolset that are loaded in the level, oldest first, so earlier\n        edits can be undone after a restart.\n\n        Returns:\n            One entry per height or paint patch actor.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.list_edits",
      "inputSchema": {
        "type": "object"
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "title": "EditInfo",
              "type": "object",
              "properties": {
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
                "priority": {
                  "type": "number",
                  "description": "Higher applies later (on top)."
                },
                "layer": {
                  "type": "string",
                  "description": "Edit layer the patch is bound to; \"-Null-\" means unbound."
                },
                "actor": {
                  "type": "string",
                  "description": "Actor label in the outliner."
                },
                "kind": {
                  "type": "string",
                  "description": "height or paint."
                },
                "edit_id": {
                  "type": "string",
                  "description": "Pass to undo_edit."
                }
              },
              "required": [
                "max_y",
                "max_x",
                "min_y",
                "min_x",
                "priority",
                "layer",
                "actor",
                "kind",
                "edit_id"
              ]
            }
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns extent, height range, material, paint layers and edit layers of the landscape.",
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
              "edit_layers": {
                "type": "array",
                "items": {
                  "type": "string"
                },
                "description": "All edit layers as \"name (class)\", in layer order."
              },
              "patch_layer": {
                "type": "string",
                "description": "Landscape Patch edit layer the edits go to. Empty until the first edit creates one."
              },
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
                "description": "Loaded proxies of this landscape (World Partition cells)."
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
              },
              "landscape_count": {
                "type": "integer",
                "description": "Loaded Landscape actors in the level. Use select_landscape when more than one."
              },
              "landscape": {
                "type": "string",
                "description": "Actor label of the Landscape the tools work on."
              }
            },
            "required": [
              "edit_layers",
              "patch_layer",
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
              "min_x",
              "landscape_count",
              "landscape"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Chooses which Landscape the tools work on when the level holds several.\n\n        Args:\n            label: Actor label of a loaded Landscape, as shown in the outliner. Empty selects the\n                first loaded Landscape again.\n\n        Returns:\n            The selected landscape, like get_landscape_info.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.select_landscape",
      "inputSchema": {
        "type": "object",
        "properties": {
          "label": {
            "type": "string"
          }
        },
        "required": [
          "label"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "LandscapeInfo",
            "type": "object",
            "properties": {
              "edit_layers": {
                "type": "array",
                "items": {
                  "type": "string"
                },
                "description": "All edit layers as \"name (class)\", in layer order."
              },
              "patch_layer": {
                "type": "string",
                "description": "Landscape Patch edit layer the edits go to. Empty until the first edit creates one."
              },
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
                "description": "Loaded proxies of this landscape (World Partition cells)."
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
              },
              "landscape_count": {
                "type": "integer",
                "description": "Loaded Landscape actors in the level. Use select_landscape when more than one."
              },
              "landscape": {
                "type": "string",
                "description": "Actor label of the Landscape the tools work on."
              }
            },
            "required": [
              "edit_layers",
              "patch_layer",
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
              "min_x",
              "landscape_count",
              "landscape"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Checks whether the open level is ready for landscape edits, without changing anything:\n        a Landscape is loaded, its height readback works, Play-In-Editor is not running, and which\n        patch edit layer and paint layers exist. Call it first and fix the listed issues.\n\n        Returns:\n            ready plus the blocking issues and hints.",
      "name": "world_ground_toolset.toolsets.landscape_tools.LandscapeTools.check_landscape",
      "inputSchema": {
        "type": "object"
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "LandscapeCheck",
            "type": "object",
            "properties": {
              "notes": {
                "type": "array",
                "items": {
                  "type": "string"
                },
                "description": "Non-blocking hints."
              },
              "issues": {
                "type": "array",
                "items": {
                  "type": "string"
                },
                "description": "Blocking problems. Empty when ready."
              },
              "edits": {
                "type": "integer",
                "description": "Patch actors of this toolset in the level, see list_edits."
              },
              "play_in_editor": {
                "type": "boolean",
                "description": "True while Play-In-Editor runs; edits are refused then."
              },
              "readback_ready": {
                "type": "boolean",
                "description": "A height read at the landscape centre returned real data."
              },
              "paint_layers": {
                "type": "array",
                "items": {
                  "type": "string"
                }
              },
              "patch_layer": {
                "type": "string",
                "description": "Existing Landscape Patch edit layer, or empty when the first edit will create one."
              },
              "proxy_count": {
                "type": "integer"
              },
              "landscape_count": {
                "type": "integer"
              },
              "landscape": {
                "type": "string"
              },
              "ready": {
                "type": "boolean",
                "description": "True when edits can run right away."
              }
            },
            "required": [
              "notes",
              "issues",
              "edits",
              "play_in_editor",
              "readback_ready",
              "paint_layers",
              "patch_layer",
              "proxy_count",
              "landscape_count",
              "landscape",
              "ready"
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
