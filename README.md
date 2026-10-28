# Character Rig AI Toolset

Inspect and edit IK retargeting, skeletal structures, sockets, skin weights and Control Rig pose previews through native Unreal MCP tools.

Public documentation for the commercial Fab plugin from Corivo. This repository contains documentation only. The plugin source, binaries and project files are distributed separately through Fab.

<!-- AI_SETUP_PROMPT_START -->
## Set up with your AI assistant

Copy the whole block below into an AI assistant that can run commands, edit files on your computer,
and connect to MCP servers. It checks the installation, enables the required plugins and connects itself.

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

<!-- AI_SETUP_PROMPT_END -->

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
