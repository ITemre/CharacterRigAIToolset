# Character Rig AI Toolset

Inspect and edit IK retargeting, skeletal structures, sockets, skin weights and Control Rig pose previews through native Unreal MCP tools.

Public documentation for the commercial Fab plugin from Corivo. This repository contains documentation only. The plugin source, binaries and project files are distributed separately through Fab.

## Features

- Inspect and edit retargeter operation stacks, rig chains and native controllers.
- Read skeletal hierarchies, reference transforms and sockets.
- Inspect dominant bone weights and perform rigid binding and mesh composition.
- Measure live PIE actor, bone, socket and morph state.
- Build pose stages, calibrate controls, check poses and capture review images.

## Requirements

- Unreal Engine 5.8, Windows 64-bit editor. Other engines and platforms are not claimed.
- Engine plugins: **ControlRig**, **ToolsetRegistry**, **PythonScriptPlugin**, **IKRig**, **MeshModelingToolset**, **GeometryScripting**, **EditorScriptingUtilities**, **SequencerScripting**, **LevelSequenceEditor**, **SkeletalMeshModelingTools**, **ModelContextProtocol**.
- An HTTP MCP-compatible assistant or client. The client and any paid AI service are separate from this product.
- Editor only. The tools do not run in a packaged game.

## Installation and MCP connection

1. Install the product from Fab when available, or copy the purchased `CharacterRigToolset` plugin folder to `<Project>/Plugins/` and build the project with a C++ toolchain supported by UE 5.8.
2. Enable `CharacterRigToolset` and its dependencies under **Edit > Plugins**, plus **Model Context Protocol**. Restart the editor.
3. Start the editor's native MCP server with `-ModelContextProtocolStartServer`, or enable auto start under **Project Settings > Plugins > Model Context Protocol**.
4. Connect your client using Streamable HTTP. The default endpoint is `http://127.0.0.1:8000/mcp`; confirm the port in the editor settings. Keep it on loopback.
5. Call `list_toolsets`, then `describe_toolset` for the identifiers listed below. Use the returned input schemas for your installed engine.

```json
{"mcpServers":{"unreal":{"type":"http","url":"http://127.0.0.1:8000/mcp"}}}
```

The JSON example is a common client configuration, not a universal format. Follow your client's setup instructions.

## Toolset identifiers

- `character_rig_toolset.toolsets.pose_authoring.PoseAuthoringTools`
- `character_rig_toolset.toolsets.pose_probe.PoseProbeTools`
- `character_rig_toolset.toolsets.retarget.RetargetTools`
- `character_rig_toolset.toolsets.retarget_assets.RetargetAssetTools`
- `character_rig_toolset.toolsets.skeleton_rig.SkeletonRigTools`
- `character_rig_toolset.toolsets.skin_weights.SkinWeightTools`

See the [captured runtime schemas](RUNTIME_SCHEMAS.md) for the verified wire-format arguments.

## First workflow

Start by inspecting an existing IK Rig or Retargeter. Discover controller methods with describe_controller, edit a duplicated asset and save it explicitly. For posing, supply your own skeletal mesh and Control Rig, create a stage, calibrate controls, apply a pose and inspect all four captured views.

The [data types](DATA_TYPES.md) describe structured arguments. The [agent guide](AI_AGENT_HANDOFF.md) includes a setup prompt and working rules. The [tool reference](TOOL_REFERENCE.md) documents every exposed operation. Read [troubleshooting](TROUBLESHOOTING.md) before using the tools on production content.

## Limitations

- Meshes, animations and Control Rig assets are supplied by the buyer. No character, animation pack or third-party content is included.
- Walk and jog generation are experimental Manny-specific workflows, not a general animation generator. They require /Game/Characters/Mannequins/Meshes/SKM_Manny_Simple.
- Pose stages use /Game/PoseLab/L_PoseLab and save their level and sequence. Use a disposable project when first learning the workflow.
- Bone rebuilding can remove morph data stored only in render data. Work on duplicates.
- Pose collision and balance checks are estimates and require visual review. A successful check does not establish animation quality.

## Validation and release availability

This documentation accompanies release preparation. It is not a claim that Fab has approved this product or that visual, gameplay or performance acceptance is complete. See [VALIDATION.md](VALIDATION.md) for the latest recorded technical checks. Until a Fab product link is added here, this product has no confirmed public purchase link.

## Support

Report reproducible problems through [GitHub issues](https://github.com/ITemre/CharacterRigAIToolset/issues). Include the engine version, plugin version, tool name, minimal arguments and error text. Remove credentials and private project data from reports.

## Licence

The commercial plugin is intended for sale under the [Fab Standard License](https://www.fab.com/eula), with Personal and Professional price tiers. Public documentation does not make the plugin open source or grant rights to redistribute it. Copyright 2026 Corivo. All rights reserved.
