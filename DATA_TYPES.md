# Data types

These fields come from the plugin source. Query `describe_toolset` for the complete wire schema, including required fields and engine-reflected types.

## StageInfo

A lab stage: the posed character, its sequence and its character space in the world.

| Field | Python declaration |
| --- | --- |
| `name` | `unreal.uproperty(str)` |
| `actor` | `unreal.uproperty(unreal.SkeletalMeshActor)` |
| `sequence` | `unreal.uproperty(unreal.LevelSequence)` |
| `control_rig` | `unreal.uproperty(str)` |
| `origin` | `unreal.uproperty(unreal.Vector)` |
| `forward` | `unreal.uproperty(unreal.Vector)` |
| `right` | `unreal.uproperty(unreal.Vector)` |

## RigCalibrationInfo

Outcome of calibrating a stage's Control Rig.

| Field | Python declaration |
| --- | --- |
| `profile_path` | `unreal.uproperty(str)` |
| `controls` | `unreal.uproperty(int)` |
| `body_parts` | `unreal.uproperty(int)` |
| `without_effect` | `unreal.uproperty(unreal.Array(str))` |

## ControlMotionInfo

One control of a rig profile: the body part it moves and how.

axes reads like 'pitch+ outward (frontal)': a positive value on that axis makes that motion,
a negative value its opposite. moves reads like 'z+ up x1.00': moving the control along that
axis moves it in that direction, and the bone follows by the factor. requires lists switch
values the control needs, like 'arm_r_fk_ik_switch=True'.

| Field | Python declaration |
| --- | --- |
| `control` | `unreal.uproperty(str)` |
| `body` | `unreal.uproperty(str)` |
| `bone` | `unreal.uproperty(str)` |
| `motions` | `unreal.uproperty(unreal.Array(str))` |
| `axes` | `unreal.uproperty(unreal.Array(str))` |
| `moves` | `unreal.uproperty(unreal.Array(str))` |
| `requires` | `unreal.uproperty(unreal.Array(str))` |

## PoseApplyInfo

Measured outcome of a transient pose preview, encoded as JSON.

| Field | Python declaration |
| --- | --- |
| `report_json` | `unreal.uproperty(str)` |
| `image_path` | `unreal.uproperty(str)` |

## RetargetOpInfo

One entry of an IK Retargeter's op stack.

| Field | Python declaration |
| --- | --- |
| `index` | `unreal.uproperty(int)` |
| `name` | `unreal.uproperty(str)` |
| `settings_type` | `unreal.uproperty(str, meta={'ToolTip': 'Name of the settings struct, which identifies the kind of op. Empty when the op exposes no settings.'})` |
| `enabled` | `unreal.uproperty(bool)` |
| `parent_op` | `unreal.uproperty(str, meta={'ToolTip': 'Name of the op this one runs under. Empty for a top-level op.'})` |

## RetargetChainInfo

A retarget chain defined on an IK Rig.

| Field | Python declaration |
| --- | --- |
| `name` | `unreal.uproperty(str)` |
| `start_bone` | `unreal.uproperty(str)` |
| `end_bone` | `unreal.uproperty(str)` |
| `goal` | `unreal.uproperty(str, meta={'ToolTip': 'IK goal at the end of the chain. Empty for an FK-only chain.'})` |

## VirtualBoneInfo

A virtual bone of a skeleton: follows the target bone, expressed as a child of the source bone.

| Field | Python declaration |
| --- | --- |
| `name` | `unreal.uproperty(str)` |
| `source_bone` | `unreal.uproperty(str)` |
| `target_bone` | `unreal.uproperty(str)` |

## SkeletonSocketInfo

A socket stored on a skeleton: shared by every skeletal mesh that uses the skeleton.

| Field | Python declaration |
| --- | --- |
| `name` | `unreal.uproperty(str)` |
| `bone` | `unreal.uproperty(str)` |
| `object_path` | `unreal.uproperty(str)` |

