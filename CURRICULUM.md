# A Rust Curriculum

A structured path to learning Rust, tailored for someone with a strong
mathematics / data science background, ~12 years of programming experience
(Python + R), and no formal CS education. The emphasis is deliberately skewed:
we move fast over things you already know (syntax, control flow, functions) and
slow down hard on the three things Rust asks of you that Python and R never did.

## What's actually new for you

You have written code for over a decade, so most of a beginner Rust book will be
review. Budget your energy for the parts that have **no analog** in Python or R:

1. **Ownership, borrowing, and lifetimes** — Rust's memory model. This is the
   single biggest conceptual jump. There is no garbage collector; the compiler
   tracks who owns each value and when it's freed. Expect to fight the borrow
   checker for a few weeks, then have it "click."
2. **The trait system** — Rust's answer to polymorphism. It replaces both
   duck typing and class inheritance with something closer to Haskell's
   typeclasses. Coming from Python's `ABC`/protocols and R's S4/S3, this will
   feel both familiar and alien.
3. **Memory layout & the stack/heap distinction** — because you don't have a
   formal CS background, invest here. Stack vs. heap, pointers, `Box`, `Vec`
   layout, and why `Copy` vs. `Move` exists. This is the one place your
   background has a genuine gap, and it happens to be the crux of Rust. Time
   spent here pays off everywhere else.

Everything else (generics, error handling, iterators, closures) will feel
natural given your background — you already think in types and higher-order
functions.

---

## Phase 0 — Setup & orientation (a few hours)

- Install the toolchain: `rustup`, `cargo`, `rust-analyzer` in your editor.
- Read the official [Learn Rust](https://rust-lang.org/learn/) landing page to
  see the lay of the land.
- Skim **Comprehensive Rust** (Google) Day 1 as a fast-paced flyover to
  calibrate: <https://google.github.io/comprehensive-rust/>. It's built for
  experienced engineers switching to Rust, so the pacing will suit you.

## Phase 1 — Core language, done properly (3–5 weeks)

**Primary text: *The Rust Programming Language* ("The Book"), 3rd edition
(covers the Rust 2024 edition).** Free online: <https://doc.rust-lang.org/book/>

Read it, but at variable speed. Suggested pacing:

| Chapters | Topic | How to approach |
|---|---|---|
| 1–3 | Setup, guessing game, common concepts | **Skim.** You know this. Read for syntax only. |
| **4** | **Understanding Ownership** | **Slow. Read twice.** The most important chapter in the book. |
| 5–6 | Structs, enums, pattern matching | Normal pace. Enums + pattern matching are a highlight — more powerful than anything in Python/R. |
| 7–8 | Modules, collections | Normal pace. |
| **9** | **Error handling (`Result`, `?`, panics)** | **Slow.** No exceptions in Rust. This is a mindset shift from `try/except`. |
| **10** | **Generics, Traits, Lifetimes** | **Slow. Read twice.** Traits are the whole type system; lifetimes are where the borrow checker gets explicit. |
| 11 | Writing tests | Normal. Rust's built-in test story is excellent. |
| 12 | CLI project (minigrep) | **Do it, don't just read it.** First real program. |
| 13 | Iterators & closures | Normal — will feel like a nicer version of Python generators / R's functional tools. |
| 14 | Cargo & crates.io | Skim, reference later. |
| **15** | **Smart pointers (`Box`, `Rc`, `RefCell`)** | **Slow.** This is where the memory model becomes concrete. |
| 16 | Fearless concurrency | Normal — but notice there's no GIL. This is a Rust superpower vs. Python. |
| 17 | Async / await | Skim on first pass; come back in Phase 4 when you need it. |
| 18–20 | OOP features, patterns, advanced | Skim; reference as needed. |
| 21 | Multithreaded web server | Optional capstone for this phase. |

**Do in parallel: Rustlings.** Small compiler-driven exercises that track the
book chapter by chapter. This is where ownership actually sinks in — reading
about the borrow checker teaches you nothing; getting yelled at by it teaches
you everything. <https://rustlings.rust-lang.org/>

**Checkpoint:** you can write a small CLI tool that reads a file, parses it,
handles errors with `Result`, and doesn't fight the borrow checker constantly.

## Phase 2 — Cement the hard parts with practice (2–4 weeks, overlaps Phase 1)

Pick **one** of these as your primary drill; both are excellent and you don't
need both:

- **100 Exercises to Learn Rust** — Luca Palmieri (author of *Zero to
  Production*). A "learn by doing" course, 100 graded exercises building a real
  project. Modern, well-sequenced, and it reinforces exactly the ownership/trait
  material. <https://rust-exercises.com/100-exercises/> — *recommended primary.*
- **Comprehensive Rust** (Google) — a full course with speaker notes and
  exercises, originally the Android team's onboarding. Good if you prefer a
  lecture-style structure. <https://google.github.io/comprehensive-rust/>

Also keep **Rust by Example** open as a lookup reference for "how do I write X":
<https://doc.rust-lang.org/rust-by-example/>

**Checkpoint:** ownership has clicked. You reach for the right container
(`Vec`, `HashMap`, `Box`, `Rc`) without thinking, and you can explain *why* a
value moved.

## Phase 3 — Understand the "why", not just the "how" (3–5 weeks)

Now that you can write working Rust, go deeper on the machinery. This is where
your no-formal-CS gap gets filled.

**Primary text: *Programming Rust*, 2nd ed. — Blandy, Orendorff, Tindall
(O'Reilly).** Where The Book teaches *how* ownership works, this explains *why*
it works that way, what the compiler is doing, and how memory is laid out. It's
the best single resource for building real mechanical intuition.

Priority chapters for you:

- **Ch. 4 — Ownership and Moves** and **Ch. 5 — References**: the definitive
  treatment. Read even though you've seen ownership already — the depth is
  different.
- **Ch. 8 — Crates and Modules** and **Ch. 9 — Structs**: memory layout of your
  own types.
- **Ch. 11 — Traits and Generics**: the deep version. Static vs. dynamic
  dispatch, trait objects, associated types. Critical.
- **Ch. 13 — Utility Traits**: `Deref`, `From`/`Into`, `Default`, etc. — the
  glue of idiomatic Rust.
- **Ch. 15 — Iterators**: how the zero-cost abstraction actually compiles away.
- **Ch. 19 — Concurrency**: threads, `Send`/`Sync`, channels, shared state.

Skim the rest. Don't force a linear read — treat chapters 6–7, 10, 12, 14,
16–18, 20–23 as reference.

**Checkpoint:** you can read the standard library docs and understand the trait
bounds; you know when a clone is cheap vs. expensive; stack vs. heap is intuitive.

## Phase 4 — Write *idiomatic*, professional Rust (ongoing)

At this point you can write correct Rust. These two make it *good* Rust — the
difference between "Python person writing Rust" and "Rust engineer."

- **Rust for Rustaceans — Jon Gjengset (No Starch).** The canonical
  "intermediate → professional" book, aimed exactly at someone 6–12 months in
  who keeps hitting the same walls. Prioritize:
  - Ch. 1 — Foundations (ownership/lifetimes/variance, revisited with rigor)
  - Ch. 2 — Types (memory layout, alignment, trait coherence)
  - Ch. 3 — Designing Interfaces (how to build good APIs)
  - Ch. 4 — Error Handling (patterns for libraries vs. applications)
  - Then 5–8 (project structure, testing, macros, async) as needed.
  - His YouTube channel ("Jon Gjengset" / *Crust of Rust*) is the best free
    advanced-Rust video content that exists — pair it with the book.
- **Effective Rust — David Drysdale.** Short, dense, 35 specific idiomatic items
  ("Effective C++"-style). Best read *after* you've written some Rust so the
  advice lands. Free online: <https://effective-rust.com/>

**Checkpoint:** you write APIs others would enjoy using; you know when to reach
for `unsafe` and (mostly) how to avoid it.

## Phase 5 — Your domain: data, ML, and finance (ongoing, start whenever)

This is why Rust is worth it for you specifically: production-grade,
memory-safe, GIL-free performance for the hot paths your Python can't hit. Start
dabbling here as early as Phase 2 — a concrete goal accelerates everything.

**Numerical / data:**
- **Polars** — columnar DataFrames in Rust (you likely already use its Python
  binding). Learning the Rust core is a natural bridge from pandas/R
  data.frames, and its lazy engine is genuinely better than pandas.
  <https://pola.rs/> · <https://docs.pola.rs/>
- **ndarray** — Rust's NumPy: N-dimensional arrays, broadcasting, slicing,
  linear algebra. Your math background makes this immediately comfortable.
- **DataFusion / Apache Arrow** — query engine over Arrow; relevant for
  building fast financial data pipelines.

**Machine learning / AI:**
- **Candle** (Hugging Face) — minimalist deep-learning framework; good for
  serving/inference in production, and directly relevant to your AI Engineer
  role.
- **Burn** — a more full-featured, modern deep-learning framework in Rust.
- **Linfa** — the scikit-learn analog (classical ML: regression, clustering,
  SVM, etc.).

**Systems intuition (optional but valuable given no formal CS):**
- **Rust in Action — Tim McNamara (Manning).** Learns Rust through
  systems-y projects: a CPU emulator, memory-mapped files, floating-point
  encoding, raw network packets. This is the most enjoyable way to backfill the
  computer-architecture intuition a CS degree would have given you, and it makes
  you a sharper engineer independent of Rust.

**Project ideas that fit your work:**
1. Rewrite a slow Python data-cleaning or feature-engineering job in Polars
   (Rust) and benchmark the speedup.
2. A CLI that ingests market/tick data (CSV/Parquet via Arrow), computes rolling
   statistics with ndarray, and outputs a report.
3. A small Monte Carlo pricing engine — embarrassingly parallel, showcases
   fearless concurrency, and plays to your quant/math strengths.
4. Wrap a Rust hot-path as a Python module with **PyO3** / **maturin** — the
   highest-leverage pattern for an AI engineer: keep Python ergonomics, get Rust
   speed where it matters.

---

## Suggested timeline

This is a marathon; consistency beats intensity. A realistic pace at ~5–8
hrs/week:

- **Weeks 1–5:** Phase 1 (The Book + Rustlings).
- **Weeks 3–8:** Phase 2 in parallel (100 Exercises) — overlap intentionally.
- **Weeks 8–13:** Phase 3 (*Programming Rust*), plus start a Phase 5 project.
- **Month 4+:** Phase 4 (*Rust for Rustaceans* / *Effective Rust*) + real
  domain projects.

You'll be productive well before you "finish" — most people ship useful Rust
around week 6–8. Full fluency (the borrow checker becomes invisible) is more
like the 6-month mark.

## One-line summary of the reading list

1. **The Rust Programming Language** (free) — the foundation. *Read.*
2. **Rustlings** (free) — drills, in parallel with #1. *Do.*
3. **100 Exercises to Learn Rust** (free) — reinforcement. *Do.*
4. **Programming Rust**, 2nd ed. — the "why" and the mechanics. *Study.*
5. **Rust for Rustaceans** — leveling up to professional. *Study later.*
6. **Effective Rust** (free) — idiomatic polish. *Reference.*
7. Domain: **Polars, ndarray, Candle/Burn, Linfa, PyO3** — your reason to be here.
8. Optional: **Rust in Action** — backfill systems/CS intuition.

---

*Sources: [The Rust Programming Language](https://doc.rust-lang.org/book/),
[Rustlings](https://rustlings.rust-lang.org/),
[100 Exercises to Learn Rust](https://rust-exercises.com/100-exercises/),
[Comprehensive Rust (Google)](https://google.github.io/comprehensive-rust/),
[corrode.dev — Rust Learning Resources 2026](https://corrode.dev/blog/rust-learning-resources-2026/),
[Bitfield Consulting — Best Rust Books 2026](https://bitfieldconsulting.com/posts/best-rust-books),
[Effective Rust](https://effective-rust.com/), [Polars](https://pola.rs/).*
