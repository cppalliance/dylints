# runvil-lints

Dylint lints shared by the runvil project's repositories: rules that clippy
has no lint for. The library is the `build-lints` crate (dylint library
name `build_lints`).

| Lint | Default | What it flags |
|---|---|---|
| `io_try_without_context` | deny | `?` that converts a `std::io::Error` into another error type without naming the operation or path |

## Using it from a repository

Install `cargo-dylint` and `dylint-link` at the release that matches the
`dylint_linting` this library depends on (currently 6.3.2):

```sh
cargo install cargo-dylint@6.3.2 dylint-link@6.3.2
```

Name the library in the root `Cargo.toml`, pinned to a commit:

```toml
[workspace.metadata.dylint]
libraries = [{ git = "https://github.com/cppalliance/runvil-lints", rev = "<commit>" }]
```

Then run, from the repository root:

```sh
cargo dylint --all -- --locked --workspace --all-targets --all-features
```

`cargo dylint` fetches this repository, builds the library on the nightly in
`rust-toolchain.toml` (rustup installs it on first use), and checks every
workspace crate with it.

## Developing

The crate is its own workspace on the nightly in `rust-toolchain.toml`,
because it builds with `#![feature(rustc_private)]` against the
`clippy_utils` rev in `Cargo.toml`. The rev and the nightly move together,
with `cargo dylint upgrade`.

```sh
cargo test --locked                  # UI tests
cargo clippy --all-targets --locked
cargo fmt --check
```

A new lint is a module in `src/`, registered in `register_lints` in
`src/lib.rs`, with a UI test under `ui/` (`<lint>.rs` and the expected
`<lint>.stderr`).
