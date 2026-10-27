# Validation status

Checks recorded on 27 October 2026, using Unreal Engine 5.8.4 and Windows 64-bit in an isolated project. Original production projects were not edited.

| Gate | Result |
| --- | --- |
| Clean Win64 BuildPlugin with strict includes | Passed |
| Fresh editor loading and tool registration | Passed |
| Native MCP HTTP connection and tool schemas | Passed |
| Asset-independent Python subset | 77 passed, 4 skipped because they require PIE or a project asset; zero failures or errors |
| Create and inspect a fresh retargeter | Passed |
| Full character / Control Rig / skin-weight workflows | Not accepted; requires representative supplied project assets |
| Visual and gameplay acceptance | Pending where applicable |
| Performance on representative buyer content | Pending |
| Fab approval and product publication | Not submitted |

Successful compilation or automation does not establish visual quality, gameplay behaviour or performance. Mac, Linux and other engine versions have not been accepted. Expected diagnostic warnings are included in the test counts and must not be described as warning-free.

