# Vendored ZeroClaw v0.8.2

This directory is a local clone of ZeroClaw v0.8.2 used only by the
`source-runtime` Docker build target. The default `upstream-runtime` target
clones the upstream tag on demand inside the build stage and does not depend
on this directory.

## State

- Branch: `vendored-v0.8.2-native-browser` (local-only; not pushed)
- Upstream: https://github.com/zeroclaw-labs/zeroclaw
- Tag: `v0.8.2` (commit `56b5a1f75`)

## Refresh procedure

```sh
cd vendor/zeroclaw
git fetch --tags upstream
git reset --hard v0.8.2
```

## Custom patches

`.v0.7.5-custom-patches.patch` preserves the two customizations that lived in
the `patched-v0.7.5-native-browser` branch of the prior vendor (a personal
fork at https://github.com/axellpadilla/zeroclaw):

- `crates/zeroclaw-runtime/src/agent/loop_.rs` — suppress intermediate
  narrated reasoning on tool-call turns and wrap the final response in
  `append_receipt_footer`. **Subsumed in v0.8.2** by the "Duplicate
  narration guard" (PR #8014) and the receipt-footer refactor in
  `crates/zeroclaw-runtime/src/agent/agent.rs:5472`. Not re-applied.
- `crates/zeroclaw-runtime/src/tools/skill_tool.rs` — pass
  `shell_env_passthrough` env vars to skill shell commands. **Still relevant
  in v0.8.2** because the regular `shell.rs` honors `shell_env_passthrough`
  via `collect_allowed_shell_env_vars`, but `skill_tool.rs` only passes
  `SAFE_ENV_VARS` and `ZEROCLAW_SESSION_ID`. Not re-applied to keep the
  vendor tree clean; can be upstreamed as a focused PR against
  `zeroclaw-labs/zeroclaw` if the credential-passthrough behavior is
  needed in production.

## Build dependencies

The clean v0.8.2 tree is ~250 MB on disk after stripping `target/`,
`node_modules/`, and `web/dist/`. The `upstream-runtime` build stage clones
the upstream tag and does not depend on this directory.
