# Learning

Three separate learning tracks in one repo. Each has its own curriculum and its
own checklist, and none is a prerequisite for another.

| Track | Where | Pace | What it's for |
|---|---|---|---|
| **Rust** + the Lead AI year | repo root | slow, long-running | The language, plus DDIA, evals, governance, staff craft |
| **ML systems** | [`ml-systems/`](./ml-systems/) | active | Training models by hand, and systems/API design |
| **TypeScript** | [`typescript/`](./typescript/) | parked | A 3–4 week side quest ending in an agent harness |

Start with the `CHECKLIST.md` of whichever track you're working on.

## Prerequisites (Rust track)

To run the crates under `projects/`, you'll need the Rust toolchain installed:

- **Rust & Cargo:** [Install Rust](https://www.rust-lang.org/tools/install)

## Getting Started

The `projects/` directory is a Cargo workspace, so run a specific crate by name
from the repo root:

```bash
cargo run -p guessing_game
```

`cargo check --workspace` type-checks everything at once.

## What's in here

**Rust track (root):**

- **[`CHECKLIST.md`](./CHECKLIST.md)** — the tracker. What to do next, in order.
- [`CURRICULUM.md`](./CURRICULUM.md) — why each Rust resource, and how fast to read it
- [`LEAD_AI_CURRICULUM.md`](./LEAD_AI_CURRICULUM.md) — the 12-month AI engineer track
- [`PROJECTS.md`](./PROJECTS.md) — the 8-project build ladder
- `projects/` — the crates themselves

**[`ml-systems/`](./ml-systems/)** — TinyTorch → PyTorch → serving: build a
framework in NumPy, train a GPT, put it behind an API you can defend.

**[`typescript/`](./typescript/)** — 3–4 weeks of TypeScript, ending in an agent
harness.

## Rust resources

- [The Rust Programming Language (The Book)](https://doc.rust-lang.org/book/)
- [Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/)
- [Rust Standard Library Documentation](https://doc.rust-lang.org/std/)
