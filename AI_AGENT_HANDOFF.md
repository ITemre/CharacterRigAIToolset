# Character Rig AI Toolset: agent guide

## Copy this setup prompt to your assistant

```text
Please set up the "Character Rig AI Toolset" Unreal Engine plugin for me and connect yourself to it.

1. Find my Unreal project (.uproject). Ask me to select it if there are several. It must use
   Unreal Engine 5.8 on Windows. Preserve unsaved editor work before editing settings or restarting.
2. Check that the purchased plugin is installed: either in the engine under
   Engine/Plugins/Marketplace or as <Project>/Plugins/CharacterRigToolset. If it is missing, stop
   and tell me to install it from Fab first.
3. Enable these plugins in the .uproject "Plugins" list (add entries with "Enabled": true and
   preserve all other settings): CharacterRigToolset, ControlRig, ToolsetRegistry,
   PythonScriptPlugin, IKRig, MeshModelingToolset, GeometryScripting, EditorScriptingUtilities,
   SequencerScripting, LevelSequenceEditor, SkeletalMeshModelingTools, ModelContextProtocol.
4. Start the editor's native MCP server with -ModelContextProtocolStartServer, or enable auto start
   in Project Settings > Plugins > Model Context Protocol and restart safely. Confirm the server's
   configured port. The default endpoint is http://127.0.0.1:8000/mcp. Verify it belongs to this project.
5. Add that endpoint to your own MCP configuration as an HTTP (Streamable HTTP) server named
   "unreal", then reload your MCP connection if needed. Preserve existing server entries; use a
   different name if "unreal" already points to another project. Keep the endpoint on loopback.
6. Call list_toolsets and confirm every identifier below is listed. Call describe_toolset for each:
   character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools
   character_rig_toolset.toolsets.pose_probe.PoseProbeTools
   character_rig_toolset.toolsets.retarget.RetargetTools
   character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools
   character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools
   character_rig_toolset.toolsets.skin_weights.SkinWeightTools
7. Read the agent guide at
   https://github.com/ITemre/CharacterRigAIToolset/blob/main/AI_AGENT_HANDOFF.md
   (also shipped as Documentation/AI_AGENT_HANDOFF.md) and follow the tool reference and project rules.
8. Do not switch levels, create a pose stage, retarget assets or start PIE just to verify setup.
   Missing character or Control Rig assets are content prerequisites, not a connection failure.
   Tell me in two sentences which project and endpoint are connected, whether all required toolsets
   are available, and what I can ask you to do next.
```

## Working rules

- Inspect before changing content. Read `describe_toolset` rather than guessing arguments.
- Work on duplicates when changing meshes, bones, weight data or package references.
- Stop PIE before editor asset mutations; PIE probing is read-only unless explicitly documented.
- Check dirty levels before any workflow that opens or saves another level.
- Save only the intended assets. A tool success is not visual or gameplay acceptance.
- Keep asset paths and file paths distinct. `/Game/...` is a package path, not a Windows filename.
- Request operation-specific approval from your user for destructive changes or redistribution of assets.

## Posing against scene objects

Clean contact with props, walls and furniture comes from a check-and-correct loop, not from the
first guess:

1. Place the objects the character touches in the PoseLab level before posing (a crate to sit on,
   a wall to lean on). The pose checks only see geometry that is in the level.
2. Apply or save the pose, then read `checks.scene.intersections` in the report. Each entry names
   the bone, the actor and an estimated depth; `inside: true` means the depth is a lower bound.
3. Move each reported part out of the object by the reported depth plus 0.5 cm, along the
   direction it came from (a hand on a wall moves back from the wall, a pelvis on a seat moves up),
   and apply again. Repeat until `intersections` is empty; two or three rounds are usual.
4. A hand target places the wrist. With the default neutral wrist the palm keeps its rest
   orientation; add a hand `rotate` (for example `up` 80 to 140 for a palm flat on a wall in front)
   and check it in the captures. Finger groups such as `fingers_r` curl whole fingers.
5. Look at the four captures before accepting the pose. Fingers have no collision proxies, so
   finger contact is a visual check.

## Product-specific constraints

- Meshes, animations and Control Rig assets are supplied by the buyer. No character, animation pack or third-party content is included.
- Walk and jog generation are experimental Manny-specific workflows, not a general animation generator. They require /Game/Characters/Mannequins/Meshes/SKM_Manny_Simple.
- Pose stages use /Game/PoseLab/L_PoseLab and save their level and sequence. Use a disposable project when first learning the workflow.
- Bone rebuilding can remove morph data stored only in render data. Work on duplicates.
- Pose collision and balance checks are estimates and require visual review. A successful check does not establish animation quality.
