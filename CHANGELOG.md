# Changelog

## 1.0.2

- Package for the Fab technical review: source only, without `Binaries` (Fab compiles the plugin), and without `Documentation/LICENSE.md`. The plugin is covered by the Fab Standard License alone.
- Pure plugin: internal test code (automation tests, Python test suites and test runners) is no longer shipped. No levels or sample assets are included.

## 1.0.1

Pose authoring learned three things from posing real scenes (sitting on a crate, pushing it,
carrying a box, reaching up a wall) with an AI agent:

- Neutral wrists: a hand with a position target now keeps its wrist as in the initial pose, so a
  moved hand no longer arrives bent at the wrist. Hand rotate motions apply on top of the neutral
  wrist, and `"wrist": "free"` restores the previous behaviour. The report lists `wrists`.
- Reach assistance: when a position target stalls, the solver recruits the clavicle on the hand's
  side and then the two upper spine segments before giving up, as a person leans into a reach.
  Recruited controls are reported under `assists`; the error text names them when a target still
  cannot be reached. The iteration budget grew from 24 to 40.
- Finger groups: `fingers_l`, `fingers_r` and one group per finger (`index_r`, `thumb_l`, ...)
  take a single `bend` or `straighten` amount for every calibrated finger segment. A fingertip
  without its own measured hinge follows the hinge axis of the segment below it. `get_body_parts`
  lists the groups.
- Scene geometry check: pose checks now test the character's collision proxies against static
  meshes in the level (props, crates, walls) and report each penetration with the bone, the actor
  and an estimated depth, as a warning in `checks.scene` and `checks.warnings`. A hand placed into
  a wall is measured, not left to the eye. The lab floor stays with the ground check.
- Fixed: the floor below the stage was found with a trace starting 10 m above the character, so
  any object over the character (a crate it sits on, a box in its hands, a roof) was taken for the
  floor and shifted ground contacts, foot anchors and the balance estimate. The trace now starts
  just above the soles.

Unchanged: tool names, MCP identifiers, native modules and the saved rig profile format.

## 1.0.0 release candidate

- Prepare a standalone Corivo Fab package from the 2026-10-18 backup.
- Add public documentation and explicit engine/plugin requirements.
- Keep native module names and MCP identifiers stable.

- Improve pose capture exposure and hide sky backdrops that can add capture warnings.
- Complete mesh, retarget, Control Rig, animation and PIE validation.
