# Buzz agent guidance

This fork contains the Rust relay/CLI/agent harness, Tauri desktop, web client, and Flutter mobile app. Before non-trivial work read [VISION.md](VISION.md), relevant `VISION_*.md`, and [TESTING.md](TESTING.md). Load the applicable retained sections: [protocol/architecture](AGENT-DETAILS.md#key-patterns), [review invariants](AGENT-DETAILS.md#review-proven-rules), [desktop](AGENT-DETAILS.md#desktop-app), [mobile](AGENT-DETAILS.md#mobile-app-flutter), [screenshots](AGENT-DETAILS.md#writing-e2e-screenshot-specs), [schema/bootstrap gotchas](AGENT-DETAILS.md#common-gotchas), and [release ecosystem](AGENT-DETAILS.md#ecosystem). These are task routes, not a requirement to read the entire companion.

## Gates

Activate `. ./bin/activate-hermit` before Git, hooks, or checks so the pinned tools win; do not rewrite hooks around an unconfigured PATH. Run `just ci` before each PR. Flutter/Dart-only changes may use `just mobile-install mobile-check mobile-test`; native/build changes also require platform checks. Relay/database/auth changes require `just test` with Postgres and Redis. `just test-unit` requires no infrastructure. Root Cargo tests exclude desktop: use the Tauri manifest or desktop recipes too. Commit with `git commit -s`; keep DCO trailers through rebases/cherry-picks.

## Critical constraints

- No unsafe Rust or new production `unwrap()`/`expect()`; document public APIs. Failures propagate or leave durable retry records. Fence async results by generation, bound resources/process trees, preserve atomic persistence, and keep recovery affordances available.
- Nostr kinds are centralized in `buzz-core/src/kind.rs`. Preserve host-derived community boundaries and correct `h` versus addressable `d` scoping; prefer event kinds to new HTTP APIs. Agent operations belong in `buzz-cli`.
- Desktop caches reset through `resetCommunityState()`. Use named rem typography tokens. Preserve mention identity, literal-label/caret behavior, keyboard/pointer semantics, and accessibility.
- Mobile uses Riverpod/hooks, shared-only feature dependencies, structured logging, and enforced file-size ceilings; split oversized files instead of bypassing guards.
- UI regression tests must bind production seams. Desktop screenshot specs require the E2E bridge build and animation waits; use repository screenshot hosting procedures only when publication is authorized.

Read [RELEASING.md](RELEASING.md) for source/tag/signing/deployment boundaries. Green CI does not prove a deployed workflow; preserve live communities and running services during validation.
