# Learn-by-Doing: A Rust Project Ladder

A sequence of projects that escalate in difficulty **in lockstep with
[`CURRICULUM.md`](./CURRICULUM.md)**. Each one is chosen to force you to use the
concepts you're reading about at that moment, and — wherever possible — it's
drawn from your world (finance, data, ML) so the payoff is real.

**How to use this:** don't read ahead and don't build ahead. Start a project
when you reach its phase. Each entry lists the *concepts it forces you to learn*,
the *crates* involved, a *build path* (milestones so you're never staring at a
blank `main.rs`), *stretch goals* for when it's too easy, and a *definition of
done* so you know when to move on.

Put each project in its own crate under `projects/` (you already have
`hello_cargo` and `guessing_game` there). The repo root is a Cargo workspace
that globs `projects/*`, so a new crate joins it automatically — run this from
the root:

```bash
cargo new projects/interest_calculator
```

Then `cargo run -p interest_calculator` to run it, or `cargo check --workspace`
to type-check everything at once.

---

## Project 1 — Compound Interest & Loan Amortization CLI
**Phase 1 · Book ch. 1–9 · difficulty: ★☆☆☆☆**

A command-line tool: given principal, annual rate, term, and compounding
frequency, print the future value and a year-by-year amortization/growth table.

- **Forces you to learn:** functions, `struct`s, `enum`s (e.g. `Compounding::{Monthly, Quarterly, Annual}`), `match`, parsing strings to numbers, `Result` + the `?` operator, `println!` formatting (`{:.2}`, column alignment). This is your first real encounter with "no exceptions — handle the error."
- **Crates:** none yet (stdlib only — do it the hard way first), then optionally `clap` for argument parsing.
- **Build path:**
  1. Hardcode the inputs; compute future value; print it.
  2. Add the amortization loop and a formatted table.
  3. Read inputs from `std::env::args()`; parse them; return a `Result` from `main`.
  4. Handle bad input gracefully (negative rate, unparseable number) instead of panicking.
- **Stretch:** add a `--csv` flag that writes the schedule to a file; support extra monthly payments and show interest saved.
- **Done when:** invalid input produces a friendly message (not a panic/backtrace), and the amortization table is correct against a spreadsheet you build to check it.

## Project 2 — CSV Stats Summarizer
**Phase 1–2 · Book ch. 8, 9, 13 · difficulty: ★★☆☆☆**

Point it at a CSV of numbers and it prints per-column count, mean, median, std
dev, min, max — a tiny `df.describe()`. This is the project that makes iterators
and error handling click.

- **Forces you to learn:** `Vec`, `HashMap`, iterators and iterator adaptors (`map`, `filter`, `fold`, `sum`, `collect`), ownership of `String` vs `&str`, and real error handling across malformed rows. The `Display` trait for pretty output.
- **Crates:** first pass with stdlib only; second pass refactor to the `csv` crate and see how much cleaner it gets.
- **Build path:**
  1. Read a file to a `String`, split into lines and fields.
  2. Parse a single numeric column; compute mean and min/max via iterators.
  3. Generalize to all columns; store results in a `HashMap<String, ColumnStats>`.
  4. Skip/collect malformed rows instead of crashing; report how many were skipped.
  5. Refactor to the `csv` crate.
- **Stretch:** infer column types (numeric vs. text); add median/quantiles; benchmark against pandas `describe()` on a big file (you'll be pleasantly surprised).
- **Done when:** it handles a messy real CSV (missing values, a header row, a stray text column) without panicking and the stats match pandas.

## Project 3 — Portfolio Ledger
**Phase 2 · Book ch. 7, 10, 11, 14 · difficulty: ★★★☆☆**

A CLI that records buy/sell transactions, tracks holdings and cost basis, and
reports realized/unrealized P&L. Persists to a JSON file between runs.

- **Forces you to learn:** modules (splitting code across files), **generics and traits** for real, a **custom error type** (an `enum` implementing `std::error::Error`), the `From` trait for error conversion, `serde` for (de)serialization, and writing **unit tests** (`#[test]`) as you go.
- **Crates:** `serde` + `serde_json`, `thiserror` (or hand-roll the error enum first to feel the pain), optionally `chrono` for dates.
- **Build path:**
  1. Model `Transaction` and `Holding` types; split into modules (`ledger`, `error`, `report`).
  2. Implement add-transaction and recompute-holdings logic; write tests for cost-basis math *first* (you're a quant — TDD the numbers).
  3. Define a custom `LedgerError` enum; make functions return `Result<_, LedgerError>`.
  4. Save/load the ledger as JSON with `serde`.
  5. Add a `report` subcommand: holdings table + P&L.
- **Stretch:** FIFO vs. average-cost basis as a trait with two implementations (this is the trait-based polymorphism lesson made concrete); add a `--fetch-prices` flag later once you learn async (Project 6).
- **Done when:** you can add transactions across multiple runs, the JSON round-trips, cost-basis tests pass, and the code is split cleanly across modules with a real error type.

## Project 4 — k-Nearest-Neighbors Classifier, From Scratch
**Phase 3 · Book ch. 10, 13 + *Programming Rust* ch. 11, 15 · difficulty: ★★★☆☆**

Implement kNN on a small labeled dataset (Iris is the classic). No ML crate —
you write the distance metric, the sorting, the voting.

- **Forces you to learn:** generics with **trait bounds** (a `Distance` trait; generic over the point type), lifetimes when returning references into your dataset, heavier iterator chains, and the zero-cost-abstraction intuition *Programming Rust* is teaching you. Your math background makes the algorithm trivial — the point is the Rust.
- **Crates:** stdlib; optionally `ndarray` for the vector math once it works.
- **Build path:**
  1. Load the dataset (reuse your Project 2 CSV skills).
  2. Define a `Distance` trait; implement Euclidean and Manhattan.
  3. Implement `k`-nearest lookup with an iterator pipeline + partial sort.
  4. Majority-vote the label; evaluate accuracy with a train/test split.
  5. Make the classifier generic over the distance metric via a trait bound.
- **Stretch:** swap the naive O(n) search for a k-d tree; port the vector ops to `ndarray` and compare ergonomics.
- **Done when:** accuracy matches scikit-learn's `KNeighborsClassifier` on the same split, and swapping the distance metric requires no changes to the core algorithm (proof your trait design is right).

## Project 5 — Monte Carlo Option Pricer (parallel)
**Phase 3 · Book ch. 13, 16 + *Programming Rust* ch. 19 · difficulty: ★★★★☆**

Price a European option by Monte Carlo simulation of geometric Brownian motion,
then make it fly by running the paths in parallel. This is the project that
shows off why Rust beats your Python here: **no GIL**, real threads, near-linear
speedup.

- **Forces you to learn:** closures, RNG, **fearless concurrency** (`std::thread` first, then `rayon`), `Send`/`Sync` in practice, and performance measurement. Plays directly to your quant/math strengths so the domain never blocks the learning.
- **Crates:** `rand` + `rand_distr`, then `rayon` for the parallel version.
- **Build path:**
  1. Single-threaded: simulate one GBM path to expiry; payoff; discount.
  2. Average over N paths for the price; sanity-check against the Black–Scholes closed form (you know it — that's the reference oracle).
  3. Parallelize with `std::thread` + chunked work and a channel to collect results.
  4. Replace all that with `rayon`'s `par_iter()` and marvel at the diff.
  5. Benchmark 1 vs. all cores; report the speedup.
- **Stretch:** add variance reduction (antithetic variates); price a path-dependent (Asian/barrier) option; add greeks by finite differences.
- **Done when:** the MC price converges to Black–Scholes within Monte Carlo error, and the `rayon` version scales roughly linearly with core count (benchmark it).

## Project 6 — Async Market-Data Fetcher
**Phase 4 · Book ch. 17 (async) + Rust for Rustaceans ch. 8 · difficulty: ★★★★☆**

A tool that fetches quotes/OHLC for a list of tickers concurrently from a public
API and writes them out. This is where you meet `async`/`await` for real —
concurrency for I/O-bound work, the complement to Project 5's CPU-bound threads.

- **Forces you to learn:** the `async`/`await` model, the `tokio` runtime, futures and `join`/`join_all` for concurrent requests, JSON deserialization of responses, and error handling across network failures.
- **Crates:** `tokio`, `reqwest`, `serde`/`serde_json`, `anyhow` for app-level errors.
- **Build path:**
  1. Fetch one ticker; deserialize the JSON into a typed struct.
  2. Fetch a list sequentially; then concurrently with `futures::join_all`.
  3. Handle rate limits / failures per-ticker without aborting the whole batch.
  4. Write results to CSV/JSON (reuse earlier projects).
  5. Wire it into your Portfolio Ledger (Project 3) as the `--fetch-prices` feature.
- **Stretch:** add a retry-with-backoff wrapper; a small TUI dashboard with `ratatui`; stream live updates over a websocket.
- **Done when:** fetching N tickers is meaningfully faster concurrently than sequentially, and one failing ticker doesn't kill the run.

## Project 7 — Rust Hot-Path as a Python Module (PyO3)
**Phase 4–5 · difficulty: ★★★★☆**

The highest-leverage project for an AI Engineer: take the Monte Carlo pricer
(Project 5) or a feature-engineering function, compile it as a native Python
extension, `import` it from Python, and benchmark against the pure-Python
version. This is the pattern you'll actually use at work — keep Python
ergonomics, drop into Rust for the hot 5%.

- **Forces you to learn:** FFI boundaries, the `#[pyfunction]`/`#[pymodule]` macros, converting between Rust and Python types, releasing the GIL for the parallel section, and packaging/building with `maturin`.
- **Crates:** `pyo3`, `maturin`, plus `rayon` (from Project 5).
- **Build path:**
  1. `maturin new`; expose a trivial `add(a, b)` function; `import` it in Python.
  2. Port the Monte Carlo pricer; call it from Python; verify it matches.
  3. Release the GIL around the parallel MC loop so it scales in Python too.
  4. Benchmark: pure Python vs. NumPy-vectorized vs. your Rust module.
  5. Write it up — this is a genuinely portfolio-worthy result.
- **Stretch:** accept/return NumPy arrays via `numpy`/`rust-numpy`; expose the kNN classifier (Project 4) with a scikit-learn-like `.fit()/.predict()` API.
- **Done when:** `import your_module` works from a normal venv and the Rust version is dramatically faster than pure Python on a real workload — with a benchmark to prove it.

## Project 8 — Capstone: Mini Backtesting Engine
**Phase 5 · difficulty: ★★★★★**

Tie it all together. Ingest historical price data (Parquet/Arrow), compute
signals with rolling statistics, run a simple strategy over the data, and report
performance (returns, Sharpe, max drawdown). Everything you've learned shows up
here: types, traits, error handling, iterators, concurrency, data crates.

- **Forces you to learn:** **Polars** and/or **ndarray** at scale, designing a clean multi-module architecture, trait-based strategy plug-ins, and end-to-end error handling on real, messy data.
- **Crates:** `polars` (lazy API), `ndarray`, `chrono`, plotting via `plotters` (optional), `clap`.
- **Build path:**
  1. Load a Parquet/CSV of daily bars into a Polars `DataFrame`.
  2. Compute indicators (moving averages, rolling vol) with Polars expressions.
  3. Define a `Strategy` trait; implement a baseline (e.g. moving-average crossover).
  4. Run the backtest loop; track positions, equity curve, and P&L.
  5. Report metrics; optionally plot the equity curve with `plotters`.
- **Stretch:** parameter sweep in parallel with `rayon`; multiple strategies compared; walk-forward validation; expose the whole thing to Python via PyO3 (Project 7).
- **Done when:** it runs end-to-end on real historical data, a new strategy is just a new `impl Strategy`, and the reported metrics match a reference you compute in pandas/NumPy.

---

## The through-line

Notice the deliberate reuse: your CSV skills (P2) feed the classifier (P4) and
backtester (P8); the Monte Carlo engine (P5) becomes a Python module (P7); the
price fetcher (P6) plugs into the ledger (P3). By the capstone you're not
learning syntax anymore — you're composing a system, which is exactly the point
where a language stops being a subject and becomes a tool.

**If you're also running [`LEAD_AI_CURRICULUM.md`](./LEAD_AI_CURRICULUM.md):**
its eval runner is the year's spine project, and this ladder becomes the gym —
Projects 1–2 as warm-ups, Project 6 as the async drill before eval runner v2,
Projects 5 → 7 as the work-relevant electives, and 3/4/8 as optional.

Track your progress in [`CHECKLIST.md`](./CHECKLIST.md).
