# Character Rig AI Toolset: agent guide

## Copy this setup prompt to your assistant

```text
Set up the purchased Character Rig AI Toolset plugin for my Unreal Engine 5.8 project.
Find the .uproject and ask me to select it if there are several. Verify the plugin
is already installed through Fab or in Project/Plugins/CharacterRigToolset.
Enable the plugin and its declared dependencies without replacing other settings.
Enable ModelContextProtocol, restart when necessary, and connect to the editor's
native Streamable HTTP MCP server on its configured loopback port.
Run list_toolsets and describe_toolset; verify these identifiers:
character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools
character_rig_toolset.toolsets.pose_probe.PoseProbeTools
character_rig_toolset.toolsets.retarget.RetargetTools
character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools
character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools
character_rig_toolset.toolsets.skin_weights.SkinWeightTools
Read https://github.com/ITemre/CharacterRigAIToolset/blob/main/README.md and the tool reference.
Start with read-only inspection in a disposable project and show me the first result.
```

## Working rules

- Inspect before changing content. Read `describe_toolset` rather than guessing arguments.
- Work on duplicates when changing meshes, bones, weight data or package references.
- Stop PIE before editor asset mutations; PIE probing is read-only unless explicitly documented.
- Check dirty levels before any workflow that opens or saves another level.
- Save only the intended assets. A tool success is not visual or gameplay acceptance.
- Keep asset paths and file paths distinct. `/Game/...` is a package path, not a Windows filename.
- Request operation-specific approval from your user for destructive changes or redistribution of assets.

## Product-specific constraints

- Meshes, animations and Control Rig assets are supplied by the buyer. No character, animation pack or third-party content is included.
- Walk and jog generation are experimental Manny-specific workflows, not a general animation generator. They require /Game/Characters/Mannequins/Meshes/SKM_Manny_Simple.
- Pose stages use /Game/PoseLab/L_PoseLab and save their level and sequence. Use a disposable project when first learning the workflow.
- Bone rebuilding can remove morph data stored only in render data. Work on duplicates.
- Pose collision and balance checks are estimates and require visual review. A successful check does not establish animation quality.
