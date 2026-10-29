# Validation status

Practical checks recorded on 27 to 29 October 2026, using Unreal Engine 5.8.4 / Win64 in isolated local projects. Production projects and the original backup ZIPs were unchanged.

## Version 1.0.2 (29 October 2026)

- Package for the Fab technical review: source only. No Binaries, no LICENSE file, no test code, no levels or sample assets.
- Clean Win64 BuildPlugin with strict includes on the unpacked release ZIP, as Fab builds it: passed, no compiler warnings.
- Buyer install path: the plugin built from the ZIP was installed into a fresh Blueprint-only project; the editor mounted it, registered all six toolsets and answered `list_toolsets`, `describe_toolset` and `list_poses` over the native MCP server.
- The test suites used for earlier checks are internal and are no longer part of the package.

## 1.0.1 (28 October 2026)

Python-only change set (pose authoring and pose checks); the native CharacterRigPreview module is the 1.0.0 build.

- Full plugin suite in the editor: 203 tests, 198 passed, 0 failed, 5 skipped (retarget fixtures and a live PIE session, as in 1.0.0).
- New tests cover the neutral wrist (within 1 degree of the initial wrist, released with "wrist": "free"), hand rotations on top of the neutral wrist, finger groups curling every calibrated segment of one hand only, a far reach that recruits the clavicle, a near reach that does not, and scene geometry penetration reported with bone, actor and depth and cleared when the object moves away.
- The floor trace fix was found by the new scene test: with a box over the stage origin the old trace reported the box top as the floor.
- Practical poses on the Manny stage with their props in the level: sitting on a crate, pushing a crate with flat palms, both hands flat on a wall overhead. Each pose was corrected from the scene report until no penetration remained (one or two rounds), saved, exported as a verified AnimSequence and rendered. Pointing at full reach now solves with the clavicle recruited.
- Known limits: a kneeling pose with the hand at knee height stays unreachable for this rig and is reported with the recruited controls named. Fingers have no collision proxies, so finger contact remains a visual check; a two-handed box carry was not convincing enough to show.
- Walk and jog cycle presets built and verified unchanged.

## Technical and practical checks

- Clean Win64 BuildPlugin with strict includes: passed.
- Fresh editor loading, native MCP tool discovery and schema reads: passed.
- All 194 distinct existing test cases covered: 189 in the full editor suite, plus the 5 previously skipped cases with prepared retarget assets and a live PIE session. Zero test failures.
- After the preview lighting and sky-backdrop fix, all 14 capture regression tests passed again, with zero engine errors during that run.
- Real Manny/Control Rig stages, four-angle captures, retarget chains, goals and a live player pawn were checked.
- The fixed experimental walk and jog presets both created and verified animation assets from the official Manny fixture. These remain Manny-specific experiments, not a general animation generator.
- The initial full-suite runner had a final log-path reporting error because the test process used a custom log location. Its raw report is preserved; test coverage was verified independently from the complete unittest log and the five successful follow-ups.

## Scope of acceptance

These are reproducible technical and visual smoke checks on the named fixtures. They do not claim exhaustive character compatibility or performance at arbitrary project scale. Mac, Linux, other engine versions, large production Landscapes and all possible third-party rigs are outside the validated scope. Third-party example assets used locally for testing are not included in the product package.

## Fab status

Prepared as a release candidate. Product approval, submission and publication are separate from these tests.
