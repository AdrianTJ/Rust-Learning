# Rust track: workspace

A Cargo workspace: every crate under `projects/` joins it automatically. Steps
are in [`../CHECKLIST.md`](../CHECKLIST.md), section **R**.

Run everything from this folder:

```bash
cargo new projects/interest_calculator   # a new project
cargo run -p guessing_game               # run one crate
cargo check --workspace                  # type-check everything
```

Toolchain: [rustup](https://www.rust-lang.org/tools/install).
