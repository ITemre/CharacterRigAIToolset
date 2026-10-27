# Validation status

Practical checks recorded on 27 October 2026, using Unreal Engine 5.8.4 / Win64 in isolated local projects. Production projects and the original backup ZIPs were unchanged.

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
