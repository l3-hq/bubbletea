# Bubble Tea fork

- Upstream: https://github.com/charmbracelet/bubbletea
- Base revision: v2.1.1 (`67b4c7a8a2b34a0b1ce15261b0c705bc7e1a04b7`)
- Strategy: upstream-first
- Patch budget: 200 lines
- Sync cadence: check upstream and rebase at least every 30 days; assess security fixes immediately

This fork carries one library patch. CI, module-path, and toolchain commits on `main` after v2.1.1 are not library patches. The module path is `github.com/laith-page/bubbletea/v2`. To use a tagged fork without a `replace`, update imports and the dependency to that path. Packages compiled against `charm.land/bubbletea/v2` use distinct Go types and must be updated or adapted when combined with this fork.

## Patches

| Commit | Change | Reason |
|---|---|---|
| [`bd63332`](https://github.com/laith-page/bubbletea/commit/bd63332403bae15732a02a1b7ee53913eb4b8ae8) Wait for renderer shutdown before restarting its ticker | `tea.go` joins the renderer goroutine after its ticker stops, before a restart resets it, and guards a renderer that never started. `renderer_lifecycle_test.go` covers the stop/restart race. | The old renderer's `Stop` could race the restarted ticker's `Reset` and leave rendering stopped. |

That commit is the box/runs pin. Its library diff against upstream v2.1.1 is those two files (120 insertions, 3 deletions). No other library, example, or dependency patch is part of the fork delta.

## Sync cadence

Check upstream and sync at least every 30 days; assess security fixes immediately. Keep the patch upstream-first: upstream it or drop it when an upstream release supplies the fix. A patch older than 180 days without an upstream pull request requires upstreaming, dropping it, or replacing it with a first-party module.
