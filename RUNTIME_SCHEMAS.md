# Runtime MCP schemas

Captured from `describe_toolset` in the native Unreal Engine 5.8.4 MCP server. These are documentation examples, not plugin source files. Refresh the schema from your own editor before automation.

## character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools

```json
{
  "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools",
  "version": "1.0",
  "description": "Authors character poses on a lab stage: a character in the lab level, bound in a level\n    sequence with a Control Rig track and turned to face world +X. Positions are given in\n    character space with named axes (forward, right, up, in cm). A rig profile, measured by\n    calibration, tells which control moves which body part and how.",
  "tools": [
    {
      "description": "Lists the controls of the stage's rig profile by body part, with the motions they make.\n        Calibrates first when no profile is saved yet.\n\n        Args:\n            stage_name: Stage name given to ensure_stage.\n            body_filter: Case-insensitive text the body part name has to contain, like 'arm_r' or\n                'spine'. Empty lists every body part.\n\n        Returns:\n            One entry per control, sorted by body part.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.get_body_parts",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "body_filter": {
            "type": "string"
          }
        },
        "required": [
          "stage_name",
          "body_filter"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "title": "ControlMotionInfo",
              "type": "object",
              "properties": {
                "requires": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  }
                },
                "moves": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  }
                },
                "axes": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  }
                },
                "motions": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  }
                },
                "bone": {
                  "type": "string"
                },
                "body": {
                  "type": "string"
                },
                "control": {
                  "type": "string"
                }
              },
              "required": [
                "requires",
                "moves",
                "axes",
                "motions",
                "bone",
                "body",
                "control"
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
      "description": "Measures what every control of the stage's Control Rig does and saves the result as\n        the rig profile of that rig and mesh. Takes about ten seconds. Writes no keys and leaves\n        the sequence unchanged. Run it again after the rig or the mesh changed.\n\n        Args:\n            stage_name: Stage name given to ensure_stage.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.calibrate_rig",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          }
        },
        "required": [
          "stage_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "RigCalibrationInfo",
            "type": "object",
            "properties": {
              "without_effect": {
                "type": "array",
                "items": {
                  "type": "string"
                }
              },
              "body_parts": {
                "type": "integer"
              },
              "controls": {
                "type": "integer"
              },
              "profile_path": {
                "type": "string"
              }
            },
            "required": [
              "without_effect",
              "body_parts",
              "controls",
              "profile_path"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns an existing stage without building or changing anything.\n\n        Args:\n            name: Stage name given to ensure_stage.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.get_stage",
      "inputSchema": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string"
          }
        },
        "required": [
          "name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "StageInfo",
            "type": "object",
            "properties": {
              "right": {
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
              },
              "forward": {
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
              },
              "origin": {
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
              },
              "control_rig": {
                "type": "string"
              },
              "sequence": {
                "type": "object",
                "title": "/Script/LevelSequence.LevelSequence",
                "description": "Represents a reference to a UObject or UClass.",
                "properties": {
                  "refPath": {
                    "type": "string",
                    "description": "The reference stored as a soft path string."
                  }
                },
                "required": [
                  "refPath"
                ]
              },
              "actor": {
                "type": "object",
                "title": "/Script/Engine.SkeletalMeshActor",
                "description": "Represents a reference to a UObject or UClass.",
                "properties": {
                  "refPath": {
                    "type": "string",
                    "description": "The reference stored as a soft path string."
                  }
                },
                "required": [
                  "refPath"
                ]
              },
              "name": {
                "type": "string"
              }
            },
            "required": [
              "right",
              "forward",
              "origin",
              "control_rig",
              "sequence",
              "actor",
              "name"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Builds the stage called name, or reuses it when it exists, and saves it. Opens the lab\n        level /Game/PoseLab/L_PoseLab and fails instead of discarding unsaved changes in the\n        level that is open.\n\n        Args:\n            name: Stage name: letters, digits and underscores, starting with a letter.\n            mesh: The skeletal mesh to pose.\n            control_rig: The Control Rig asset that poses it.\n\n        Returns:\n            The stage with its character space: origin on the ground, forward and right as\n            world directions.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.ensure_stage",
      "inputSchema": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string"
          },
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "control_rig": {
            "type": "object",
            "title": "/Script/ControlRigDeveloper.ControlRigBlueprint",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "name",
          "mesh",
          "control_rig"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "StageInfo",
            "type": "object",
            "properties": {
              "right": {
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
              },
              "forward": {
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
              },
              "origin": {
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
              },
              "control_rig": {
                "type": "string"
              },
              "sequence": {
                "type": "object",
                "title": "/Script/LevelSequence.LevelSequence",
                "description": "Represents a reference to a UObject or UClass.",
                "properties": {
                  "refPath": {
                    "type": "string",
                    "description": "The reference stored as a soft path string."
                  }
                },
                "required": [
                  "refPath"
                ]
              },
              "actor": {
                "type": "object",
                "title": "/Script/Engine.SkeletalMeshActor",
                "description": "Represents a reference to a UObject or UClass.",
                "properties": {
                  "refPath": {
                    "type": "string",
                    "description": "The reference stored as a soft path string."
                  }
                },
                "required": [
                  "refPath"
                ]
              },
              "name": {
                "type": "string"
              }
            },
            "required": [
              "right",
              "forward",
              "origin",
              "control_rig",
              "sequence",
              "actor",
              "name"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Reports elbow/knee angles, sole contacts, collision proxy intersections and estimated\n        static balance of the current rig pose. Does not change controls or write keys/assets.\n        Coverage and approximation methods are included; unchecked joints and geometry are explicit.\n\n        Args:\n            stage_name: Stage name given to ensure_stage. Open its sequence before inspecting a preview.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.check_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          }
        },
        "required": [
          "stage_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Saves framed front, right-side, top and three-quarter PNGs and a combined image.\n        Captures the current visible pose without evaluating the sequence or changing controls.\n        Rig shapes, editor helpers and other pose stages are excluded from the image.\n\n        Args:\n            stage_name: Stage name given to ensure_stage. Open its sequence with get_stage first.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.capture_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          }
        },
        "required": [
          "stage_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Applies a complete pose as a transient editor preview, without keys or asset saves.\n        Each apply starts from initial controls. Errors restore the previous controls and bones.\n        Sequencer evaluation replaces the preview; apply again after moving the timeline.\n\n        Args:\n            stage_name: Stage name given to ensure_stage.\n            pose_json: JSON with version=1, base='initial' and parts keyed by get_body_parts names.\n                Example: {\"version\":1,\"base\":\"initial\",\"parts\":{\"lower_arm_r\":{\"rotate\":{\"bend\":60}}}}.\n                rotate maps motions to nonnegative offsets in degrees (0..180), relative to\n                initial controls, not absolute anatomical angles. target needs forward, right,\n                up in character-space cm and places a hand, foot, pelvis or root bone origin.\n                Empty parts resets the initial pose. Optional contacts lists feet planted at their\n                initial ground anchors, e.g. [\"foot_l\", \"foot_r\"]. Elbow/knee limits are enforced;\n                collision proxy penetration and estimated static balance are reported as warnings.\n\n        Returns:\n            JSON report with controls, checks and framed front/side/top/three-quarter captures.\n            image_path points to the combined PNG; per-view PNG paths are in the report.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.apply_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "pose_json": {
            "type": "string"
          }
        },
        "required": [
          "stage_name",
          "pose_json"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Exports a saved pose as a new static AnimSequence: one interval at 30 fps, two identical keys.\n        Verifies all mesh bones in source, compressed and actual mesh playback; includes playback views.\n        Writes no keys to the working sequence and never overwrites an existing animation asset.\n        Args:\n            stage_name: Compatible stage to solve the saved spec on.\n            pose_name: Name previously stored with save_pose.\n            asset_path: New /Game package path, e.g. /Game/PoseLab/Poses/A_Crouch.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.export_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "pose_name": {
            "type": "string"
          },
          "asset_path": {
            "type": "string"
          }
        },
        "required": [
          "stage_name",
          "pose_name",
          "asset_path"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Applies a saved pose to its compatible rig/mesh, producing fresh checks and four views.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.load_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "pose_name": {
            "type": "string"
          }
        },
        "required": [
          "stage_name",
          "pose_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Lists saved pose names, descriptions and asset compatibility; corrupt files include errors.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.list_poses",
      "inputSchema": {
        "type": "object"
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Reads a saved pose document and its rig/mesh metadata without changing the editor.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.get_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "pose_name": {
            "type": "string"
          }
        },
        "required": [
          "pose_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Checks/applies a spec and stores its canonical JSON in the project's PoseLibrary folder.\n        Rig and mesh identity are stored with the spec. Existing names require explicit replacement.\n        A failed file write restores the previous preview; invalid poses are not published.\n        Args:\n            stage_name: Existing stage to validate the pose on.\n            pose_name: Letters, digits and underscores, starting with a letter, at most 64 characters.\n            pose_json: Version-1 pose document as used by apply_pose.\n            description: Optional description; pass an empty string for none.\n            replace_existing: Pass false to preserve an existing name, true to explicitly replace it.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.save_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "pose_name": {
            "type": "string"
          },
          "pose_json": {
            "type": "string"
          },
          "description": {
            "type": "string"
          },
          "replace_existing": {
            "type": "boolean"
          }
        },
        "required": [
          "stage_name",
          "pose_name",
          "pose_json",
          "description",
          "replace_existing"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Builds a neutral in-place walking experiment on an existing stage, to a new AnimSequence.\n        Samples 32 frames at 30 fps with alternating feet, counter-swinging arms and body motion.\n        Each sample passes pose checks; source, compressed data and mesh playback are compared.\n        The working sequence stays unkeyed. Existing assets and experiment sources are preserved.\n        This fixed preset is an experiment, not the general M7 animation-spec interface.\n        Args:\n            stage_name: Existing compatible Manny stage; use a separate test stage.\n            asset_path: New /Game package path, e.g. /Game/PoseLab/WalkCycles/A_WalkCycle_Test.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.create_walk_cycle",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "asset_path": {
            "type": "string"
          }
        },
        "required": [
          "stage_name",
          "asset_path"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Builds a fixed in-place jogging experiment on an existing Manny stage.\n        Samples 24 frames at 30 fps with counter-swinging bent arms and brief flight phases.\n        All samples are checked; the new asset is verified and captured from actual playback.\n        Existing assets are preserved. The report includes per-step timings.\n        Args:\n            stage_name: Existing compatible Manny stage; use a separate test stage.\n            asset_path: New /Game package path, e.g. /Game/PoseLab/JogCycles/A_JogCycle_Test.",
      "name": "character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools.create_jog_cycle",
      "inputSchema": {
        "type": "object",
        "properties": {
          "stage_name": {
            "type": "string"
          },
          "asset_path": {
            "type": "string"
          }
        },
        "required": [
          "stage_name",
          "asset_path"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "PoseApplyInfo",
            "type": "object",
            "properties": {
              "image_path": {
                "type": "string"
              },
              "report_json": {
                "type": "string"
              }
            },
            "required": [
              "image_path",
              "report_json"
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

## character_rig_toolset.toolsets.pose_probe.PoseProbeTools

```json
{
  "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools",
  "version": "1.0",
  "description": "Measures the live pose of characters in the running Play-In-Editor world: finds play\n    world actors and what is attached to them, and reads bone and socket transforms of their\n    components, in world space or relative to another bone. For checking retargeting, IK and\n    attachments against what is actually on screen.",
  "tools": [
    {
      "description": "Returns the morph target weights currently set on a skeletal mesh component.\n\n        Args:\n            component: The component to read.\n            morph_names: Morph targets to read. A morph the mesh does not have reads 0.\n\n        Returns:\n            Morph target name mapped to its weight.",
      "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools.get_morph_weights",
      "inputSchema": {
        "type": "object",
        "properties": {
          "component": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMeshComponent",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "morph_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "component",
          "morph_names"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
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
      "description": "Returns the current transforms of bones or sockets expressed in the space of another\n        bone or socket, which may live on a different component.\n\n        Args:\n            base_component: The component that carries the reference bone or socket.\n            base_socket: Bone or socket that defines the space. Empty uses the component itself.\n            component: The component to read.\n            socket_names: Bone or socket names. Empty names read the component itself.\n\n        Returns:\n            Name mapped to its transform relative to the reference, translations in cm.",
      "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools.get_relative_transforms",
      "inputSchema": {
        "type": "object",
        "properties": {
          "base_component": {
            "type": "object",
            "title": "/Script/Engine.SceneComponent",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "base_socket": {
            "type": "string"
          },
          "component": {
            "type": "object",
            "title": "/Script/Engine.SceneComponent",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "base_component",
          "base_socket",
          "component",
          "socket_names"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
              "type": "object",
              "title": "ToolsetTransform",
              "properties": {
                "location": {
                  "description": "The world-space location.",
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
                },
                "rotation": {
                  "description": "The world-space rotation.",
                  "title": "Rotator",
                  "type": "object",
                  "properties": {
                    "pitch": {
                      "type": "number",
                      "description": "Pitch (degrees) around Y axis"
                    },
                    "yaw": {
                      "type": "number",
                      "description": "Yaw (degrees) around Z axis"
                    },
                    "roll": {
                      "type": "number",
                      "description": "Roll (degrees) around X axis"
                    }
                  },
                  "required": [
                    "pitch",
                    "yaw",
                    "roll"
                  ]
                },
                "scale": {
                  "description": "The scale.",
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
              }
            }
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns the current world transforms of bones or sockets on a component.\n\n        Args:\n            component: The component to read, typically a skeletal mesh component.\n            socket_names: Bone or socket names. Virtual bones count as bones.\n\n        Returns:\n            Name mapped to its world transform, translations in cm.",
      "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools.get_socket_transforms",
      "inputSchema": {
        "type": "object",
        "properties": {
          "component": {
            "type": "object",
            "title": "/Script/Engine.SceneComponent",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "component",
          "socket_names"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
              "type": "object",
              "title": "ToolsetTransform",
              "properties": {
                "location": {
                  "description": "The world-space location.",
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
                },
                "rotation": {
                  "description": "The world-space rotation.",
                  "title": "Rotator",
                  "type": "object",
                  "properties": {
                    "pitch": {
                      "type": "number",
                      "description": "Pitch (degrees) around Y axis"
                    },
                    "yaw": {
                      "type": "number",
                      "description": "Yaw (degrees) around Z axis"
                    },
                    "roll": {
                      "type": "number",
                      "description": "Roll (degrees) around X axis"
                    }
                  },
                  "required": [
                    "pitch",
                    "yaw",
                    "roll"
                  ]
                },
                "scale": {
                  "description": "The scale.",
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
              }
            }
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns the actors that hang on an actor: those its child actor components spawned\n        and those attached to it, including everything below them.\n\n        Args:\n            actor: The actor to start from.\n\n        Returns:\n            Child and attached actors without the actor itself.",
      "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools.get_related_actors",
      "inputSchema": {
        "type": "object",
        "properties": {
          "actor": {
            "type": "object",
            "title": "/Script/Engine.Actor",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "actor"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "type": "object",
              "title": "/Script/Engine.Actor",
              "properties": {
                "refPath": {
                  "type": "string",
                  "description": "The reference stored as a soft path string."
                }
              },
              "required": [
                "refPath"
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
      "description": "Returns the pawn the first local player controls in the running play world.",
      "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools.get_player_pawn",
      "inputSchema": {
        "type": "object"
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "title": "/Script/Engine.Pawn",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns actors of the running play world.\n\n        Args:\n            name_contains: Case-insensitive text the actor's name or class name has to contain.\n                Empty returns every actor.\n\n        Returns:\n            Matching actors, empty if none match.",
      "name": "character_rig_toolset.toolsets.pose_probe.PoseProbeTools.find_play_actors",
      "inputSchema": {
        "type": "object",
        "properties": {
          "name_contains": {
            "type": "string"
          }
        },
        "required": [
          "name_contains"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "type": "object",
              "title": "/Script/Engine.Actor",
              "properties": {
                "refPath": {
                  "type": "string",
                  "description": "The reference stored as a soft path string."
                }
              },
              "required": [
                "refPath"
              ]
            }
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

## character_rig_toolset.toolsets.retarget.RetargetTools

```json
{
  "name": "character_rig_toolset.toolsets.retarget.RetargetTools",
  "version": "1.0",
  "description": "Inspects and edits IK Retargeter assets and the IK Rigs they map between: the op stack\n    with each op's settings, chain mappings, retarget chains and IK goals.",
  "tools": [
    {
      "description": "Returns the IK goals of an IK Rig.\n\n        Args:\n            ik_rig: The IK Rig asset.\n\n        Returns:\n            Goal name mapped to the bone the goal sits on.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.get_ik_rig_goals",
      "inputSchema": {
        "type": "object",
        "properties": {
          "ik_rig": {
            "type": "object",
            "title": "/Script/IKRig.IKRigDefinition",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "ik_rig"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
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
      "description": "Returns the retarget chains of an IK Rig.\n\n        Args:\n            ik_rig: The IK Rig asset.\n\n        Returns:\n            One entry per chain.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.get_ik_rig_chains",
      "inputSchema": {
        "type": "object",
        "properties": {
          "ik_rig": {
            "type": "object",
            "title": "/Script/IKRig.IKRigDefinition",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "ik_rig"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "title": "RetargetChainInfo",
              "type": "object",
              "properties": {
                "goal": {
                  "type": "string",
                  "description": "IK goal at the end of the chain. Empty for an FK-only chain."
                },
                "end_bone": {
                  "type": "string"
                },
                "start_bone": {
                  "type": "string"
                },
                "name": {
                  "type": "string"
                }
              },
              "required": [
                "goal",
                "end_bone",
                "start_bone",
                "name"
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
      "description": "Returns which source chain drives each target chain in one op.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_name: Name of an op that maps chains.\n\n        Returns:\n            Target chain name mapped to source chain name. Unmapped target chains map to 'None'.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.get_chain_mapping",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_name": {
            "type": "string"
          }
        },
        "required": [
          "retargeter",
          "op_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
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
      "description": "Moves an op to another position in the stack.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_name: Name of the op in the stack.\n            target_index: Position the op ends up at.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.move_op",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_name": {
            "type": "string"
          },
          "target_index": {
            "type": "integer"
          }
        },
        "required": [
          "retargeter",
          "op_name",
          "target_index"
        ]
      }
    },
    {
      "description": "Removes an op from the stack.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_name: Name of the op in the stack.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.remove_op",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_name": {
            "type": "string"
          }
        },
        "required": [
          "retargeter",
          "op_name"
        ]
      }
    },
    {
      "description": "Appends an op to the end of the stack.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_type: Path of the op struct, e.g. '/Script/IKRig.IKRetargetPinBoneOp'.\n            op_name: Name for the new op. The stack makes it unique if it is taken.\n\n        Returns:\n            The op as it was added.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.add_op",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_type": {
            "type": "string"
          },
          "op_name": {
            "type": "string"
          }
        },
        "required": [
          "retargeter",
          "op_type",
          "op_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "title": "RetargetOpInfo",
            "type": "object",
            "properties": {
              "parent_op": {
                "type": "string",
                "description": "Name of the op this one runs under. Empty for a top-level op."
              },
              "enabled": {
                "type": "boolean"
              },
              "settings_type": {
                "type": "string",
                "description": "Name of the settings struct, which identifies the kind of op. Empty when the op exposes no settings."
              },
              "name": {
                "type": "string"
              },
              "index": {
                "type": "integer"
              }
            },
            "required": [
              "parent_op",
              "enabled",
              "settings_type",
              "name",
              "index"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Turns an op on or off.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_name: Name of the op in the stack.\n            enabled: Whether the op runs.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.set_op_enabled",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_name": {
            "type": "string"
          },
          "enabled": {
            "type": "boolean"
          }
        },
        "required": [
          "retargeter",
          "op_name",
          "enabled"
        ]
      }
    },
    {
      "description": "Changes the settings of one op.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_name: Name of the op in the stack.\n            settings: Settings in Unreal's text export format. Members left out keep their value.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.set_op_settings",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_name": {
            "type": "string"
          },
          "settings": {
            "type": "string"
          }
        },
        "required": [
          "retargeter",
          "op_name",
          "settings"
        ]
      }
    },
    {
      "description": "Returns the settings of one op.\n\n        Args:\n            retargeter: The retargeter asset.\n            op_name: Name of the op in the stack.\n\n        Returns:\n            The settings struct in Unreal's text export format.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.get_op_settings",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "op_name": {
            "type": "string"
          }
        },
        "required": [
          "retargeter",
          "op_name"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns the retargeter's op stack in execution order.\n\n        Args:\n            retargeter: The retargeter asset.\n\n        Returns:\n            One entry per op.",
      "name": "character_rig_toolset.toolsets.retarget.RetargetTools.get_ops",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter": {
            "type": "object",
            "title": "/Script/IKRig.IKRetargeter",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "retargeter"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "title": "RetargetOpInfo",
              "type": "object",
              "properties": {
                "parent_op": {
                  "type": "string",
                  "description": "Name of the op this one runs under. Empty for a top-level op."
                },
                "enabled": {
                  "type": "boolean"
                },
                "settings_type": {
                  "type": "string",
                  "description": "Name of the settings struct, which identifies the kind of op. Empty when the op exposes no settings."
                },
                "name": {
                  "type": "string"
                },
                "index": {
                  "type": "integer"
                }
              },
              "required": [
                "parent_op",
                "enabled",
                "settings_type",
                "name",
                "index"
              ]
            }
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

## character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools

```json
{
  "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools",
  "version": "1.0",
  "description": "Direct native access to IK Rig, solver, Retargeter, op and pose APIs.\n\n    Discover exact installed-engine signatures before calling. Changes stay\n    unsaved until save_asset. No gameplay clips are modified by these tools.\n    ",
  "tools": [
    {
      "description": "Native retarget of 1-8 clips into a dedicated /Game/ review folder.\n\n        Uses the currently selected source/target meshes and retarget poses.\n        Rejects collisions before export, never replaces existing clips, does\n        not include referenced assets. No explicit save is requested by this tool.",
      "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools.export_review",
      "inputSchema": {
        "type": "object",
        "properties": {
          "retargeter_path": {
            "type": "string"
          },
          "animation_paths": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "target_folder": {
            "type": "string"
          }
        },
        "required": [
          "retargeter_path",
          "animation_paths",
          "target_folder"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Create an unsaved IK Rig (ik_rig) or Retargeter (retargeter). Never overwrite.",
      "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools.create_asset",
      "inputSchema": {
        "type": "object",
        "properties": {
          "asset_path": {
            "type": "string"
          },
          "kind": {
            "type": "string"
          }
        },
        "required": [
          "asset_path",
          "kind"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Explicitly save one edited IK Rig or Retargeter; refuses PIE.",
      "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools.save_asset",
      "inputSchema": {
        "type": "object",
        "properties": {
          "asset_path": {
            "type": "string"
          }
        },
        "required": [
          "asset_path"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "boolean"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Read meshes, rig chains/goals/solvers or retarget poses, ops and settings.",
      "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools.inspect_asset",
      "inputSchema": {
        "type": "object",
        "properties": {
          "asset_path": {
            "type": "string"
          }
        },
        "required": [
          "asset_path"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Call a discovered native IK/retarget controller method. Does not save.\n\n        arguments_json is an object of named arguments. Native values use tags:\n        {\"$enum\":\"RetargetSourceOrTarget.TARGET\"}, {\"$asset\":\"/Game/...\"},\n        {\"$class\":\"IKRigFBIKSolver\"}, or {\"$struct\":\"Quat\",\n        \"properties\":{\"x\":0,\"y\":0,\"z\":0,\"w\":1}}. Structs also accept\n        their exported \"text\". Discover API with describe_controller first.\n        All writes refuse PIE; returned false remains false, not success.",
      "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools.call_controller",
      "inputSchema": {
        "type": "object",
        "properties": {
          "asset_path": {
            "type": "string"
          },
          "controller": {
            "type": "string"
          },
          "method": {
            "type": "string"
          },
          "arguments_json": {
            "type": "string"
          }
        },
        "required": [
          "asset_path",
          "controller",
          "method",
          "arguments_json"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Discover native methods and signatures for asset, op:<name>, or solver:<index>.\n\n        Returns JSON with read/write classification and full installed API docs.\n        Use call_controller with keyword arguments matching these signatures.",
      "name": "character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools.describe_controller",
      "inputSchema": {
        "type": "object",
        "properties": {
          "asset_path": {
            "type": "string"
          },
          "controller": {
            "type": "string"
          }
        },
        "required": [
          "asset_path",
          "controller"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
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

## character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools

```json
{
  "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools",
  "version": "1.0",
  "description": "Inspects and edits the bone setup of skeletons and skeletal meshes: virtual bones of a\n    skeleton, the bones a mesh itself carries, their reference pose, and copying or removing\n    bones on a mesh.",
  "tools": [
    {
      "description": "Removes a socket by name from a skeletal mesh and from its skeleton: the mesh-only\n        socket and the skeleton socket of that name both go.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            socket_name: Name of an existing socket.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.remove_socket",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_name": {
            "type": "string"
          }
        },
        "required": [
          "mesh",
          "socket_name"
        ]
      }
    },
    {
      "description": "Adds a socket to a skeletal mesh alone, not to its skeleton. Where a socket is looked\n        up by name on this mesh it is found before a bone of that name, so it can stand in for a\n        bone whose place does not fit this mesh. The name must not be used by a socket of the\n        mesh or its skeleton yet.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            socket_name: Name of the new socket.\n            bone_name: Bone the socket hangs on.\n            transform: Socket transform relative to the bone, translation in cm.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.add_mesh_socket",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_name": {
            "type": "string"
          },
          "bone_name": {
            "type": "string"
          },
          "transform": {
            "type": "object",
            "title": "ToolsetTransform",
            "description": "Represents a 3D transformation with optional location, rotation, and scale.\nUnset fields mean \"identity\" when creating objects and \"don't change\" when modifying existing ones.",
            "properties": {
              "location": {
                "description": "The world-space location.",
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
              },
              "rotation": {
                "description": "The world-space rotation.",
                "title": "Rotator",
                "type": "object",
                "properties": {
                  "pitch": {
                    "type": "number",
                    "description": "Pitch (degrees) around Y axis"
                  },
                  "yaw": {
                    "type": "number",
                    "description": "Yaw (degrees) around Z axis"
                  },
                  "roll": {
                    "type": "number",
                    "description": "Roll (degrees) around X axis"
                  }
                },
                "required": [
                  "pitch",
                  "yaw",
                  "roll"
                ]
              },
              "scale": {
                "description": "The scale.",
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
            }
          }
        },
        "required": [
          "mesh",
          "socket_name",
          "bone_name",
          "transform"
        ]
      }
    },
    {
      "description": "Moves a skeleton socket to another bone. Its transform relative to the bone stays as\n        it is, so the socket ends up in a different place.\n\n        Args:\n            mesh: Any skeletal mesh that uses the skeleton. Its bones validate bone_name.\n            socket_name: Name of an existing skeleton socket.\n            bone_name: Bone the socket hangs on from now on.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.set_skeleton_socket_bone",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_name": {
            "type": "string"
          },
          "bone_name": {
            "type": "string"
          }
        },
        "required": [
          "mesh",
          "socket_name",
          "bone_name"
        ]
      }
    },
    {
      "description": "Sets the transform of a skeleton socket relative to its bone.\n\n        Args:\n            mesh: Any skeletal mesh that uses the skeleton.\n            socket_name: Name of an existing skeleton socket.\n            transform: New transform, translation in cm.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.set_skeleton_socket_transform",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_name": {
            "type": "string"
          },
          "transform": {
            "type": "object",
            "title": "ToolsetTransform",
            "description": "Represents a 3D transformation with optional location, rotation, and scale.\nUnset fields mean \"identity\" when creating objects and \"don't change\" when modifying existing ones.",
            "properties": {
              "location": {
                "description": "The world-space location.",
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
              },
              "rotation": {
                "description": "The world-space rotation.",
                "title": "Rotator",
                "type": "object",
                "properties": {
                  "pitch": {
                    "type": "number",
                    "description": "Pitch (degrees) around Y axis"
                  },
                  "yaw": {
                    "type": "number",
                    "description": "Yaw (degrees) around Z axis"
                  },
                  "roll": {
                    "type": "number",
                    "description": "Roll (degrees) around X axis"
                  }
                },
                "required": [
                  "pitch",
                  "yaw",
                  "roll"
                ]
              },
              "scale": {
                "description": "The scale.",
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
            }
          }
        },
        "required": [
          "mesh",
          "socket_name",
          "transform"
        ]
      }
    },
    {
      "description": "Adds a socket to the skeleton of a skeletal mesh, so every mesh on that skeleton\n        has it. A socket with that name must not exist on the skeleton or the mesh yet.\n\n        Args:\n            mesh: Any skeletal mesh that uses the skeleton. Its bones validate bone_name.\n            socket_name: Name of the new socket.\n            bone_name: Bone the socket hangs on.\n            transform: Socket transform relative to the bone, translation in cm.\n\n        Returns:\n            The object path of the new skeleton socket.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.add_skeleton_socket",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "socket_name": {
            "type": "string"
          },
          "bone_name": {
            "type": "string"
          },
          "transform": {
            "type": "object",
            "title": "ToolsetTransform",
            "description": "Represents a 3D transformation with optional location, rotation, and scale.\nUnset fields mean \"identity\" when creating objects and \"don't change\" when modifying existing ones.",
            "properties": {
              "location": {
                "description": "The world-space location.",
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
              },
              "rotation": {
                "description": "The world-space rotation.",
                "title": "Rotator",
                "type": "object",
                "properties": {
                  "pitch": {
                    "type": "number",
                    "description": "Pitch (degrees) around Y axis"
                  },
                  "yaw": {
                    "type": "number",
                    "description": "Yaw (degrees) around Z axis"
                  },
                  "roll": {
                    "type": "number",
                    "description": "Roll (degrees) around X axis"
                  }
                },
                "required": [
                  "pitch",
                  "yaw",
                  "roll"
                ]
              },
              "scale": {
                "description": "The scale.",
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
            }
          }
        },
        "required": [
          "mesh",
          "socket_name",
          "bone_name",
          "transform"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "string"
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns the sockets stored on the skeleton of a skeletal mesh, not the mesh-only\n        sockets of the mesh itself.\n\n        Args:\n            mesh: Any skeletal mesh that uses the skeleton.\n\n        Returns:\n            One entry per socket, empty if the skeleton has none.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.get_skeleton_sockets",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "mesh"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "title": "SkeletonSocketInfo",
              "type": "object",
              "properties": {
                "object_path": {
                  "type": "string"
                },
                "bone": {
                  "type": "string"
                },
                "name": {
                  "type": "string"
                }
              },
              "required": [
                "object_path",
                "bone",
                "name"
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
      "description": "Removes bones and all their descendants from a skeletal mesh. The mesh is rebuilt\n        from its source data, so morph targets that exist only in its render data are lost.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            bone_names: Topmost bones to remove.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.remove_bones",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "mesh",
          "bone_names"
        ]
      }
    },
    {
      "description": "Copies bones with all their descendants from one skeletal mesh to another, keeping\n        parents and parent-relative transforms. Bones the target already has are left untouched.\n        The target mesh is rebuilt from its source data, so morph targets that exist only in its\n        render data are lost.\n\n        Args:\n            source_mesh: The mesh to read bones from.\n            target_mesh: The mesh that receives the bones. The parent of every copied bone must\n                exist on it.\n            bone_names: Topmost bones to copy.\n\n        Returns:\n            Names of the bones that were added, empty if the target had all of them.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.copy_bones",
      "inputSchema": {
        "type": "object",
        "properties": {
          "source_mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "target_mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "source_mesh",
          "target_mesh",
          "bone_names"
        ]
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
      "description": "Returns reference pose transforms of bones on a skeletal mesh.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            bone_names: Bones to read.\n            component_space: True for transforms relative to the mesh origin, False for transforms\n                relative to each bone's parent. Translations are in cm.\n\n        Returns:\n            Bone name mapped to its reference transform.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.get_ref_pose",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "component_space": {
            "type": "boolean"
          }
        },
        "required": [
          "mesh",
          "bone_names",
          "component_space"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
              "type": "object",
              "title": "ToolsetTransform",
              "properties": {
                "location": {
                  "description": "The world-space location.",
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
                },
                "rotation": {
                  "description": "The world-space rotation.",
                  "title": "Rotator",
                  "type": "object",
                  "properties": {
                    "pitch": {
                      "type": "number",
                      "description": "Pitch (degrees) around Y axis"
                    },
                    "yaw": {
                      "type": "number",
                      "description": "Yaw (degrees) around Z axis"
                    },
                    "roll": {
                      "type": "number",
                      "description": "Roll (degrees) around X axis"
                    }
                  },
                  "required": [
                    "pitch",
                    "yaw",
                    "roll"
                  ]
                },
                "scale": {
                  "description": "The scale.",
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
              }
            }
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns the bones a skeletal mesh itself carries, which can be fewer than its skeleton\n        asset lists. Virtual bones are not included.\n\n        Args:\n            mesh: The skeletal mesh asset.\n\n        Returns:\n            Bone names, parents before children.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.get_mesh_bones",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "mesh"
        ]
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
      "description": "Returns the virtual bones defined on a skeleton.\n\n        Args:\n            skeleton: The skeleton asset.\n\n        Returns:\n            One entry per virtual bone, empty if the skeleton has none.",
      "name": "character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools.get_virtual_bones",
      "inputSchema": {
        "type": "object",
        "properties": {
          "skeleton": {
            "type": "object",
            "title": "/Script/Engine.Skeleton",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "skeleton"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "array",
            "items": {
              "title": "VirtualBoneInfo",
              "type": "object",
              "properties": {
                "target_bone": {
                  "type": "string"
                },
                "source_bone": {
                  "type": "string"
                },
                "name": {
                  "type": "string"
                }
              },
              "required": [
                "target_bone",
                "source_bone",
                "name"
              ]
            }
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

## character_rig_toolset.toolsets.skin_weights.SkinWeightTools

```json
{
  "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools",
  "version": "1.0",
  "description": "Inspects and edits the skin weights of skeletal meshes: which bones a mesh follows,\n    binding a region of it to a single bone so a hard object that was skinned like cloth stops\n    bending with the body, and moving a mesh onto another skeleton.",
  "tools": [
    {
      "description": "Adds the geometry of a static mesh to a skeletal mesh as a part that follows a single\n        bone rigidly. The materials of the static mesh are added as new slots after the existing\n        ones, unless the mesh already has a slot of that name with that material. Gives a\n        skeleton a part its mesh lacks, such as a shell an animation moves, or\n        fixes an attachment to a mesh. Only LOD0 is changed.\n\n        Args:\n            mesh: The skeletal mesh asset to add to.\n            static_mesh: The static mesh whose LOD0 geometry and materials are added.\n            bone_name: Bone of the skeletal mesh the new part follows.\n            transform: Where the static mesh sits in the space of the skeletal mesh, in the\n                reference pose.\n\n        Returns:\n            The number of vertices added.",
      "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools.append_static_mesh",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "static_mesh": {
            "type": "object",
            "title": "/Script/Engine.StaticMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_name": {
            "type": "string"
          },
          "transform": {
            "type": "object",
            "title": "ToolsetTransform",
            "description": "Represents a 3D transformation with optional location, rotation, and scale.\nUnset fields mean \"identity\" when creating objects and \"don't change\" when modifying existing ones.",
            "properties": {
              "location": {
                "description": "The world-space location.",
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
              },
              "rotation": {
                "description": "The world-space rotation.",
                "title": "Rotator",
                "type": "object",
                "properties": {
                  "pitch": {
                    "type": "number",
                    "description": "Pitch (degrees) around Y axis"
                  },
                  "yaw": {
                    "type": "number",
                    "description": "Yaw (degrees) around Z axis"
                  },
                  "roll": {
                    "type": "number",
                    "description": "Roll (degrees) around X axis"
                  }
                },
                "required": [
                  "pitch",
                  "yaw",
                  "roll"
                ]
              },
              "scale": {
                "description": "The scale.",
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
            }
          }
        },
        "required": [
          "mesh",
          "static_mesh",
          "bone_name",
          "transform"
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
      "description": "Creates a skeletal mesh asset with the geometry and materials of a mesh, skinned to\n        another skeleton. The geometry is moved by geometry_transform, the bones take the\n        reference pose of the target skeleton, and every vertex follows the target bones its\n        source bones are mapped to. Makes a mesh usable with the animations of a skeleton it was\n        not built for. Only LOD0 is carried over, morph targets are not.\n\n        Args:\n            source_mesh: The mesh to copy geometry, materials and weights from.\n            skeleton: The skeleton the new mesh uses.\n            bone_map: Source bone name mapped to the target bone that takes over its weights.\n                Every source bone that moves vertices needs an entry; several may share a target.\n            bone_copies: Target bone mapped to another target bone whose geometry it gets a copy\n                of, for a skeleton that animates a part the source has only once, such as a\n                second magazine. The copy sits relative to its bone as the original does to its\n                own. Empty for none.\n            geometry_transform: Scale, rotation and offset applied to the geometry in mesh space\n                before it is bound, to fit it to the target skeleton. Identity leaves it in place.\n            asset_path: Package path and name of the asset, for example /Game/Weapons/SKM_New.\n                A skeletal mesh that already exists there on the same skeleton gets its geometry\n                and weights replaced and keeps its materials, sockets and references.\n\n        Returns:\n            The skeletal mesh asset, not saved yet.",
      "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools.rebind_to_skeleton",
      "inputSchema": {
        "type": "object",
        "properties": {
          "source_mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "skeleton": {
            "type": "object",
            "title": "/Script/Engine.Skeleton",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_map": {
            "type": "object",
            "additionalProperties": {
              "type": "string"
            }
          },
          "bone_copies": {
            "type": "object",
            "additionalProperties": {
              "type": "string"
            }
          },
          "geometry_transform": {
            "type": "object",
            "title": "ToolsetTransform",
            "description": "Represents a 3D transformation with optional location, rotation, and scale.\nUnset fields mean \"identity\" when creating objects and \"don't change\" when modifying existing ones.",
            "properties": {
              "location": {
                "description": "The world-space location.",
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
              },
              "rotation": {
                "description": "The world-space rotation.",
                "title": "Rotator",
                "type": "object",
                "properties": {
                  "pitch": {
                    "type": "number",
                    "description": "Pitch (degrees) around Y axis"
                  },
                  "yaw": {
                    "type": "number",
                    "description": "Yaw (degrees) around Z axis"
                  },
                  "roll": {
                    "type": "number",
                    "description": "Roll (degrees) around X axis"
                  }
                },
                "required": [
                  "pitch",
                  "yaw",
                  "roll"
                ]
              },
              "scale": {
                "description": "The scale.",
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
            }
          },
          "asset_path": {
            "type": "string"
          }
        },
        "required": [
          "source_mesh",
          "skeleton",
          "bone_map",
          "bone_copies",
          "geometry_transform",
          "asset_path"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Binds the part of a skeletal mesh that lies in one direction from a bone to that bone\n        alone, so it moves as one rigid piece. Distances are measured in the reference pose from\n        the bone along direction. Vertices beyond end follow only the bone, vertices before start\n        keep their weights, and those in between are blended. A connected part that lies entirely\n        beyond start is bound as a whole. The mesh is rebuilt from its source data, so morph\n        targets that exist only in its render data are lost. Fails without changing anything on\n        a mesh whose vertices cannot be matched to its skin weights, which happens when its\n        source data holds vertices that have to be split to read the geometry.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            bone_name: Bone the region is bound to.\n            direction: Direction in mesh space that points from the bone into the region.\n            start: Distance in cm where the blend towards the bone begins.\n            end: Distance in cm from which vertices follow only the bone. Must not be below start.\n\n        Returns:\n            The number of vertices whose weights changed.",
      "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools.bind_rigid",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_name": {
            "type": "string"
          },
          "direction": {
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
          },
          "start": {
            "type": "number"
          },
          "end": {
            "type": "number"
          }
        },
        "required": [
          "mesh",
          "bone_name",
          "direction",
          "start",
          "end"
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
      "description": "Returns where the surface of a mesh crosses a plane at a distance along a direction\n        from a bone: the points at which the edges of its triangles pass through the plane, in\n        that bone's space in the reference pose. Together they trace the outline of the mesh at\n        that height however coarse its vertices are. For the shape of a part something has to\n        fit around: the grip of a weapon, cut at the height of a finger.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            bone_name: Bone whose space the direction, the distance and the result are in.\n            direction: Direction in the bone's space the plane is perpendicular to.\n            distance: Distance in cm from the bone along direction at which the plane lies.\n\n        Returns:\n            The crossing points in the bone's space in cm, empty if the plane misses the mesh.",
      "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools.get_cross_section",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_name": {
            "type": "string"
          },
          "direction": {
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
          },
          "distance": {
            "type": "number"
          }
        },
        "required": [
          "mesh",
          "bone_name",
          "direction",
          "distance"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
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
          }
        },
        "required": [
          "returnValue"
        ]
      }
    },
    {
      "description": "Returns how far the part of a mesh that a bone moves reaches around that bone: the\n        box around the vertices the bone has the largest weight on, in the bone's own space in\n        the reference pose. For a finger bone it gives the length of the segment along the bone\n        and its thickness to each side, which the bone positions alone do not tell.\n\n        Args:\n            mesh: The skeletal mesh asset.\n            bone_names: Bones to measure.\n\n        Returns:\n            Bone name mapped to the box in that bone's space, in cm. Bones that lead no vertex\n            are left out.",
      "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools.get_bone_extents",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          },
          "bone_names": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        },
        "required": [
          "mesh",
          "bone_names"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
              "title": "Box",
              "type": "object",
              "properties": {
                "min": {
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
                },
                "max": {
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
                },
                "isValid": {
                  "type": "boolean"
                }
              },
              "required": [
                "min",
                "max",
                "isValid"
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
      "description": "Returns the bones a skeletal mesh follows, by how many vertices each one moves most.\n\n        Args:\n            mesh: The skeletal mesh asset.\n\n        Returns:\n            Bone name mapped to the number of LOD0 vertices it has the largest weight on. Bones\n            that lead no vertex are left out.",
      "name": "character_rig_toolset.toolsets.skin_weights.SkinWeightTools.get_dominant_bones",
      "inputSchema": {
        "type": "object",
        "properties": {
          "mesh": {
            "type": "object",
            "title": "/Script/Engine.SkeletalMesh",
            "description": "Represents a reference to a UObject or UClass.",
            "properties": {
              "refPath": {
                "type": "string",
                "description": "The reference stored as a soft path string."
              }
            },
            "required": [
              "refPath"
            ]
          }
        },
        "required": [
          "mesh"
        ]
      },
      "outputSchema": {
        "type": "object",
        "properties": {
          "returnValue": {
            "type": "object",
            "additionalProperties": {
              "type": "integer"
            }
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

