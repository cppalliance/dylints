# runvil-lints

The `build-lints` dylint library: custom lints shared by the runvil project's repositories, which name it in their root `[workspace.metadata.dylint]`.

## Rules

- The library builds only on the nightly in `rust-toolchain.toml`, the one the `clippy_utils` rev in `Cargo.toml` targets. Both move together, with `cargo dylint upgrade`.
- A new lint is a module in `src/`, registered by hand in `register_lints` in `src/lib.rs`, never through `dylint_linting::declare_late_lint!`, which declares a whole library around one lint.
- Every lint has a UI test under `ui/`: the input `<lint>.rs` with both flagged and allowed shapes, and the expected `<lint>.stderr`.
- Lint messages are designed assuming model consumption: concise, factual, and self-contained, naming what is unmet and how to fix it.
- Comments explain a non-obvious constraint, ordering requirement, or workaround.

## Verification

- UI tests: `cargo test --locked`.
- Linter: `cargo clippy --all-targets --locked` with `CARGO_BUILD_WARNINGS=deny`.
- Formatter: `cargo fmt --check`.
