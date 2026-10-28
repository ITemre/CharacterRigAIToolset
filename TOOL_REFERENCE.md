# Character Rig AI Toolset: tool reference

Operation names and descriptions below are taken from the selected plugin source. Query `describe_toolset` for the exact runtime schema, required fields and defaults. C++ metadata can alter wire names; use the runtime schema. Python toolset names retain their module and class names.

## PoseAuthoringTools

### `create_jog_cycle`

```text
stage_name: str, asset_path: str
```

Builds a fixed in-place jogging experiment on an existing Manny stage.
Samples 24 frames at 30 fps with counter-swinging bent arms and brief flight phases.
All samples are checked; the new asset is verified and captured from actual playback.
Existing assets are preserved. The report includes per-step timings.
Args:
    stage_name: Existing compatible Manny stage; use a separate test stage.
    asset_path: New /Game package path, e.g. /Game/PoseLab/JogCycles/A_JogCycle_Test.

### `create_walk_cycle`

```text
stage_name: str, asset_path: str
```

Builds a neutral in-place walking experiment on an existing stage, to a new AnimSequence.
Samples 32 frames at 30 fps with alternating feet, counter-swinging arms and body motion.
Each sample passes pose checks; source, compressed data and mesh playback are compared.
The working sequence stays unkeyed. Existing assets and experiment sources are preserved.
This fixed preset is an experiment, not the general M7 animation-spec interface.
Args:
    stage_name: Existing compatible Manny stage; use a separate test stage.
    asset_path: New /Game package path, e.g. /Game/PoseLab/WalkCycles/A_WalkCycle_Test.

### `save_pose`

```text
stage_name: str, pose_name: str, pose_json: str, description: str, replace_existing: bool
```

Checks/applies a spec and stores its canonical JSON in the project's PoseLibrary folder.
Rig and mesh identity are stored with the spec. Existing names require explicit replacement.
A failed file write restores the previous preview; invalid poses are not published.
Args:
    stage_name: Existing stage to validate the pose on.
    pose_name: Letters, digits and underscores, starting with a letter, at most 64 characters.
    pose_json: Version-1 pose document as used by apply_pose.
    description: Optional description; pass an empty string for none.
    replace_existing: Pass false to preserve an existing name, true to explicitly replace it.

### `get_pose`

```text
pose_name: str
```

Reads a saved pose document and its rig/mesh metadata without changing the editor.

### `list_poses`

```text

```

Lists saved pose names, descriptions and asset compatibility; corrupt files include errors.

### `load_pose`

```text
stage_name: str, pose_name: str
```

Applies a saved pose to its compatible rig/mesh, producing fresh checks and four views.

### `export_pose`

```text
stage_name: str, pose_name: str, asset_path: str
```

Exports a saved pose as a new static AnimSequence: one interval at 30 fps, two identical keys.
Verifies all mesh bones in source, compressed and actual mesh playback; includes playback views.
Writes no keys to the working sequence and never overwrites an existing animation asset.
Args:
    stage_name: Compatible stage to solve the saved spec on.
    pose_name: Name previously stored with save_pose.
    asset_path: New /Game package path, e.g. /Game/PoseLab/Poses/A_Crouch.

### `apply_pose`

```text
stage_name: str, pose_json: str
```

Applies a complete pose as a transient editor preview, without keys or asset saves.
Each apply starts from initial controls. Errors restore the previous controls and bones.
Sequencer evaluation replaces the preview; apply again after moving the timeline.

Args:
    stage_name: Stage name given to ensure_stage.
    pose_json: JSON with version=1, base='initial' and parts keyed by get_body_parts names.
        Example: {"version":1,"base":"initial","parts":{"lower_arm_r":{"rotate":{"bend":60}}}}.
        rotate maps motions to nonnegative offsets in degrees (0..180), relative to
        initial controls, not absolute anatomical angles. target needs forward, right,
        up in character-space cm and places a hand, foot, pelvis or root bone origin.
        A targeted hand keeps a neutral wrist (its rotate motions then bend the wrist from
        neutral); "wrist":"free" keeps the control's own orientation instead. When a
        target stalls, the solver recruits the clavicle and then the upper spine; the
        report lists them under assists. Finger groups pose whole fingers with one bend
        or straighten amount: "fingers_r", "fingers_l", or one finger like "index_r".
        Empty parts resets the initial pose. Optional contacts lists feet planted at their
        initial ground anchors, e.g. ["foot_l", "foot_r"]. Elbow/knee limits are enforced;
        collision proxy penetration, penetration of scene geometry (props, crates, walls in
        the level, with depth) and estimated static balance are reported as warnings.

Returns:
    JSON report with controls, checks and framed front/side/top/three-quarter captures.
    image_path points to the combined PNG; per-view PNG paths are in the report.

### `capture_pose`

```text
stage_name: str
```

Saves framed front, right-side, top and three-quarter PNGs and a combined image.
Captures the current visible pose without evaluating the sequence or changing controls.
Rig shapes, editor helpers and other pose stages are excluded from the image.

Args:
    stage_name: Stage name given to ensure_stage. Open its sequence with get_stage first.

### `check_pose`

```text
stage_name: str
```

Reports elbow/knee angles, sole contacts, collision proxy intersections, penetration of
scene geometry (static meshes such as props, crates and walls, with an estimated depth per
bone) and estimated static balance of the current rig pose. Does not change controls or
write keys/assets.
Coverage and approximation methods are included; unchecked joints and geometry are explicit.

Args:
    stage_name: Stage name given to ensure_stage. Open its sequence before inspecting a preview.

### `ensure_stage`

```text
name: str, mesh: unreal.SkeletalMesh, control_rig: unreal.ControlRigBlueprint
```

Builds the stage called name, or reuses it when it exists, and saves it. Opens the lab
level /Game/PoseLab/L_PoseLab and fails instead of discarding unsaved changes in the
level that is open.

Args:
    name: Stage name: letters, digits and underscores, starting with a letter.
    mesh: The skeletal mesh to pose.
    control_rig: The Control Rig asset that poses it.

Returns:
    The stage with its character space: origin on the ground, forward and right as
    world directions.

### `get_stage`

```text
name: str
```

Returns an existing stage without building or changing anything.

Args:
    name: Stage name given to ensure_stage.

### `calibrate_rig`

```text
stage_name: str
```

Measures what every control of the stage's Control Rig does and saves the result as
the rig profile of that rig and mesh. Takes about ten seconds. Writes no keys and leaves
the sequence unchanged. Run it again after the rig or the mesh changed.

Args:
    stage_name: Stage name given to ensure_stage.

### `get_body_parts`

```text
stage_name: str, body_filter: str
```

Lists the controls of the stage's rig profile by body part, with the motions they make.
Calibrates first when no profile is saved yet.

Args:
    stage_name: Stage name given to ensure_stage.
    body_filter: Case-insensitive text the body part name has to contain, like 'arm_r' or
        'spine'. Empty lists every body part.

Returns:
    One entry per control, sorted by body part. Finger groups (fingers_l, fingers_r and
    one entry per finger such as index_r) are listed as well; they take bend or straighten.

## PoseProbeTools

### `find_play_actors`

```text
name_contains: str
```

Returns actors of the running play world.

Args:
    name_contains: Case-insensitive text the actor's name or class name has to contain.
        Empty returns every actor.

Returns:
    Matching actors, empty if none match.

### `get_player_pawn`

```text

```

Returns the pawn the first local player controls in the running play world.

### `get_related_actors`

```text
actor: unreal.Actor
```

Returns the actors that hang on an actor: those its child actor components spawned
and those attached to it, including everything below them.

Args:
    actor: The actor to start from.

Returns:
    Child and attached actors without the actor itself.

### `get_socket_transforms`

```text
component: unreal.SceneComponent, socket_names: list[str]
```

Returns the current world transforms of bones or sockets on a component.

Args:
    component: The component to read, typically a skeletal mesh component.
    socket_names: Bone or socket names. Virtual bones count as bones.

Returns:
    Name mapped to its world transform, translations in cm.

### `get_relative_transforms`

```text
base_component: unreal.SceneComponent, base_socket: str, component: unreal.SceneComponent, socket_names: list[str]
```

Returns the current transforms of bones or sockets expressed in the space of another
bone or socket, which may live on a different component.

Args:
    base_component: The component that carries the reference bone or socket.
    base_socket: Bone or socket that defines the space. Empty uses the component itself.
    component: The component to read.
    socket_names: Bone or socket names. Empty names read the component itself.

Returns:
    Name mapped to its transform relative to the reference, translations in cm.

### `get_morph_weights`

```text
component: unreal.SkeletalMeshComponent, morph_names: list[str]
```

Returns the morph target weights currently set on a skeletal mesh component.

Args:
    component: The component to read.
    morph_names: Morph targets to read. A morph the mesh does not have reads 0.

Returns:
    Morph target name mapped to its weight.

## RetargetTools

### `get_ops`

```text
retargeter: unreal.IKRetargeter
```

Returns the retargeter's op stack in execution order.

Args:
    retargeter: The retargeter asset.

Returns:
    One entry per op.

### `get_op_settings`

```text
retargeter: unreal.IKRetargeter, op_name: str
```

Returns the settings of one op.

Args:
    retargeter: The retargeter asset.
    op_name: Name of the op in the stack.

Returns:
    The settings struct in Unreal's text export format.

### `set_op_settings`

```text
retargeter: unreal.IKRetargeter, op_name: str, settings: str
```

Changes the settings of one op.

Args:
    retargeter: The retargeter asset.
    op_name: Name of the op in the stack.
    settings: Settings in Unreal's text export format. Members left out keep their value.

### `set_op_enabled`

```text
retargeter: unreal.IKRetargeter, op_name: str, enabled: bool
```

Turns an op on or off.

Args:
    retargeter: The retargeter asset.
    op_name: Name of the op in the stack.
    enabled: Whether the op runs.

### `add_op`

```text
retargeter: unreal.IKRetargeter, op_type: str, op_name: str
```

Appends an op to the end of the stack.

Args:
    retargeter: The retargeter asset.
    op_type: Path of the op struct, e.g. '/Script/IKRig.IKRetargetPinBoneOp'.
    op_name: Name for the new op. The stack makes it unique if it is taken.

Returns:
    The op as it was added.

### `remove_op`

```text
retargeter: unreal.IKRetargeter, op_name: str
```

Removes an op from the stack.

Args:
    retargeter: The retargeter asset.
    op_name: Name of the op in the stack.

### `move_op`

```text
retargeter: unreal.IKRetargeter, op_name: str, target_index: int
```

Moves an op to another position in the stack.

Args:
    retargeter: The retargeter asset.
    op_name: Name of the op in the stack.
    target_index: Position the op ends up at.

### `get_chain_mapping`

```text
retargeter: unreal.IKRetargeter, op_name: str
```

Returns which source chain drives each target chain in one op.

Args:
    retargeter: The retargeter asset.
    op_name: Name of an op that maps chains.

Returns:
    Target chain name mapped to source chain name. Unmapped target chains map to 'None'.

### `get_ik_rig_chains`

```text
ik_rig: unreal.IKRigDefinition
```

Returns the retarget chains of an IK Rig.

Args:
    ik_rig: The IK Rig asset.

Returns:
    One entry per chain.

### `get_ik_rig_goals`

```text
ik_rig: unreal.IKRigDefinition
```

Returns the IK goals of an IK Rig.

Args:
    ik_rig: The IK Rig asset.

Returns:
    Goal name mapped to the bone the goal sits on.

## RetargetAssetTools

### `describe_controller`

```text
asset_path: str, controller: str
```

Discover native methods and signatures for asset, op:<name>, or solver:<index>.

Returns JSON with read/write classification and full installed API docs.
Use call_controller with keyword arguments matching these signatures.

### `call_controller`

```text
asset_path: str, controller: str, method: str, arguments_json: str
```

Call a discovered native IK/retarget controller method. Does not save.

arguments_json is an object of named arguments. Native values use tags:
{"$enum":"RetargetSourceOrTarget.TARGET"}, {"$asset":"/Game/..."},
{"$class":"IKRigFBIKSolver"}, or {"$struct":"Quat",
"properties":{"x":0,"y":0,"z":0,"w":1}}. Structs also accept
their exported "text". Discover API with describe_controller first.
All writes refuse PIE; returned false remains false, not success.

### `inspect_asset`

```text
asset_path: str
```

Read meshes, rig chains/goals/solvers or retarget poses, ops and settings.

### `save_asset`

```text
asset_path: str
```

Explicitly save one edited IK Rig or Retargeter; refuses PIE.

### `create_asset`

```text
asset_path: str, kind: str
```

Create an unsaved IK Rig (ik_rig) or Retargeter (retargeter). Never overwrite.

### `export_review`

```text
retargeter_path: str, animation_paths: list[str], target_folder: str
```

Native retarget of 1-8 clips into a dedicated /Game/ review folder.

Uses the currently selected source/target meshes and retarget poses.
Rejects collisions before export, never replaces existing clips, does
not include referenced assets. No explicit save is requested by this tool.

## SkeletonRigTools

### `get_virtual_bones`

```text
skeleton: unreal.Skeleton
```

Returns the virtual bones defined on a skeleton.

Args:
    skeleton: The skeleton asset.

Returns:
    One entry per virtual bone, empty if the skeleton has none.

### `get_mesh_bones`

```text
mesh: unreal.SkeletalMesh
```

Returns the bones a skeletal mesh itself carries, which can be fewer than its skeleton
asset lists. Virtual bones are not included.

Args:
    mesh: The skeletal mesh asset.

Returns:
    Bone names, parents before children.

### `get_ref_pose`

```text
mesh: unreal.SkeletalMesh, bone_names: list[str], component_space: bool
```

Returns reference pose transforms of bones on a skeletal mesh.

Args:
    mesh: The skeletal mesh asset.
    bone_names: Bones to read.
    component_space: True for transforms relative to the mesh origin, False for transforms
        relative to each bone's parent. Translations are in cm.

Returns:
    Bone name mapped to its reference transform.

### `copy_bones`

```text
source_mesh: unreal.SkeletalMesh, target_mesh: unreal.SkeletalMesh, bone_names: list[str]
```

Copies bones with all their descendants from one skeletal mesh to another, keeping
parents and parent-relative transforms. Bones the target already has are left untouched.
The target mesh is rebuilt from its source data, so morph targets that exist only in its
render data are lost.

Args:
    source_mesh: The mesh to read bones from.
    target_mesh: The mesh that receives the bones. The parent of every copied bone must
        exist on it.
    bone_names: Topmost bones to copy.

Returns:
    Names of the bones that were added, empty if the target had all of them.

### `remove_bones`

```text
mesh: unreal.SkeletalMesh, bone_names: list[str]
```

Removes bones and all their descendants from a skeletal mesh. The mesh is rebuilt
from its source data, so morph targets that exist only in its render data are lost.

Args:
    mesh: The skeletal mesh asset.
    bone_names: Topmost bones to remove.

### `get_skeleton_sockets`

```text
mesh: unreal.SkeletalMesh
```

Returns the sockets stored on the skeleton of a skeletal mesh, not the mesh-only
sockets of the mesh itself.

Args:
    mesh: Any skeletal mesh that uses the skeleton.

Returns:
    One entry per socket, empty if the skeleton has none.

### `add_skeleton_socket`

```text
mesh: unreal.SkeletalMesh, socket_name: str, bone_name: str, transform: unreal.Transform
```

Adds a socket to the skeleton of a skeletal mesh, so every mesh on that skeleton
has it. A socket with that name must not exist on the skeleton or the mesh yet.

Args:
    mesh: Any skeletal mesh that uses the skeleton. Its bones validate bone_name.
    socket_name: Name of the new socket.
    bone_name: Bone the socket hangs on.
    transform: Socket transform relative to the bone, translation in cm.

Returns:
    The object path of the new skeleton socket.

### `set_skeleton_socket_transform`

```text
mesh: unreal.SkeletalMesh, socket_name: str, transform: unreal.Transform
```

Sets the transform of a skeleton socket relative to its bone.

Args:
    mesh: Any skeletal mesh that uses the skeleton.
    socket_name: Name of an existing skeleton socket.
    transform: New transform, translation in cm.

### `set_skeleton_socket_bone`

```text
mesh: unreal.SkeletalMesh, socket_name: str, bone_name: str
```

Moves a skeleton socket to another bone. Its transform relative to the bone stays as
it is, so the socket ends up in a different place.

Args:
    mesh: Any skeletal mesh that uses the skeleton. Its bones validate bone_name.
    socket_name: Name of an existing skeleton socket.
    bone_name: Bone the socket hangs on from now on.

### `add_mesh_socket`

```text
mesh: unreal.SkeletalMesh, socket_name: str, bone_name: str, transform: unreal.Transform
```

Adds a socket to a skeletal mesh alone, not to its skeleton. Where a socket is looked
up by name on this mesh it is found before a bone of that name, so it can stand in for a
bone whose place does not fit this mesh. The name must not be used by a socket of the
mesh or its skeleton yet.

Args:
    mesh: The skeletal mesh asset.
    socket_name: Name of the new socket.
    bone_name: Bone the socket hangs on.
    transform: Socket transform relative to the bone, translation in cm.

### `remove_socket`

```text
mesh: unreal.SkeletalMesh, socket_name: str
```

Removes a socket by name from a skeletal mesh and from its skeleton: the mesh-only
socket and the skeleton socket of that name both go.

Args:
    mesh: The skeletal mesh asset.
    socket_name: Name of an existing socket.

## SkinWeightTools

### `get_dominant_bones`

```text
mesh: unreal.SkeletalMesh
```

Returns the bones a skeletal mesh follows, by how many vertices each one moves most.

Args:
    mesh: The skeletal mesh asset.

Returns:
    Bone name mapped to the number of LOD0 vertices it has the largest weight on. Bones
    that lead no vertex are left out.

### `get_bone_extents`

```text
mesh: unreal.SkeletalMesh, bone_names: list[str]
```

Returns how far the part of a mesh that a bone moves reaches around that bone: the
box around the vertices the bone has the largest weight on, in the bone's own space in
the reference pose. For a finger bone it gives the length of the segment along the bone
and its thickness to each side, which the bone positions alone do not tell.

Args:
    mesh: The skeletal mesh asset.
    bone_names: Bones to measure.

Returns:
    Bone name mapped to the box in that bone's space, in cm. Bones that lead no vertex
    are left out.

### `get_cross_section`

```text
mesh: unreal.SkeletalMesh, bone_name: str, direction: unreal.Vector, distance: float
```

Returns where the surface of a mesh crosses a plane at a distance along a direction
from a bone: the points at which the edges of its triangles pass through the plane, in
that bone's space in the reference pose. Together they trace the outline of the mesh at
that height however coarse its vertices are. For the shape of a part something has to
fit around: the grip of a weapon, cut at the height of a finger.

Args:
    mesh: The skeletal mesh asset.
    bone_name: Bone whose space the direction, the distance and the result are in.
    direction: Direction in the bone's space the plane is perpendicular to.
    distance: Distance in cm from the bone along direction at which the plane lies.

Returns:
    The crossing points in the bone's space in cm, empty if the plane misses the mesh.

### `bind_rigid`

```text
mesh: unreal.SkeletalMesh, bone_name: str, direction: unreal.Vector, start: float, end: float
```

Binds the part of a skeletal mesh that lies in one direction from a bone to that bone
alone, so it moves as one rigid piece. Distances are measured in the reference pose from
the bone along direction. Vertices beyond end follow only the bone, vertices before start
keep their weights, and those in between are blended. A connected part that lies entirely
beyond start is bound as a whole. The mesh is rebuilt from its source data, so morph
targets that exist only in its render data are lost. Fails without changing anything on
a mesh whose vertices cannot be matched to its skin weights, which happens when its
source data holds vertices that have to be split to read the geometry.

Args:
    mesh: The skeletal mesh asset.
    bone_name: Bone the region is bound to.
    direction: Direction in mesh space that points from the bone into the region.
    start: Distance in cm where the blend towards the bone begins.
    end: Distance in cm from which vertices follow only the bone. Must not be below start.

Returns:
    The number of vertices whose weights changed.

### `rebind_to_skeleton`

```text
source_mesh: unreal.SkeletalMesh, skeleton: unreal.Skeleton, bone_map: dict[str, str], bone_copies: dict[str, str], geometry_transform: unreal.Transform, asset_path: str
```

Creates a skeletal mesh asset with the geometry and materials of a mesh, skinned to
another skeleton. The geometry is moved by geometry_transform, the bones take the
reference pose of the target skeleton, and every vertex follows the target bones its
source bones are mapped to. Makes a mesh usable with the animations of a skeleton it was
not built for. Only LOD0 is carried over, morph targets are not.

Args:
    source_mesh: The mesh to copy geometry, materials and weights from.
    skeleton: The skeleton the new mesh uses.
    bone_map: Source bone name mapped to the target bone that takes over its weights.
        Every source bone that moves vertices needs an entry; several may share a target.
    bone_copies: Target bone mapped to another target bone whose geometry it gets a copy
        of, for a skeleton that animates a part the source has only once, such as a
        second magazine. The copy sits relative to its bone as the original does to its
        own. Empty for none.
    geometry_transform: Scale, rotation and offset applied to the geometry in mesh space
        before it is bound, to fit it to the target skeleton. Identity leaves it in place.
    asset_path: Package path and name of the asset, for example /Game/Weapons/SKM_New.
        A skeletal mesh that already exists there on the same skeleton gets its geometry
        and weights replaced and keeps its materials, sockets and references.

Returns:
    The skeletal mesh asset, not saved yet.

### `append_static_mesh`

```text
mesh: unreal.SkeletalMesh, static_mesh: unreal.StaticMesh, bone_name: str, transform: unreal.Transform
```

Adds the geometry of a static mesh to a skeletal mesh as a part that follows a single
bone rigidly. The materials of the static mesh are added as new slots after the existing
ones, unless the mesh already has a slot of that name with that material. Gives a
skeleton a part its mesh lacks, such as a shell an animation moves, or
fixes an attachment to a mesh. Only LOD0 is changed.

Args:
    mesh: The skeletal mesh asset to add to.
    static_mesh: The static mesh whose LOD0 geometry and materials are added.
    bone_name: Bone of the skeletal mesh the new part follows.
    transform: Where the static mesh sits in the space of the skeletal mesh, in the
        reference pose.

Returns:
    The number of vertices added.
