# The Checklist

**The single tracker.** Everything to do, in the order to do it. This is the only
file with checkboxes and the only file that says "next."

The others are reference — consult them when a step points you there:

- [`CURRICULUM.md`](./CURRICULUM.md) — *why* each Rust resource, and how fast to read it
- [`LEAD_AI_CURRICULUM.md`](./LEAD_AI_CURRICULUM.md) — *why* each non-Rust track matters
- [`PROJECTS.md`](./PROJECTS.md) — milestones, stretch goals, and definitions of done for Projects 1–8

When a step says "Project 2, milestones 3–4," the milestones are in `PROJECTS.md`.
Don't copy them back here — keep this a checklist.

**Started:** 2026-07-25

---

## How to read a step

Each step has some mix of four line types:

- **Read** — the current spine book
- **Drill** — Rustlings / 100 Exercises, compiler-driven repetition
- **Build** — the project work
- **Write** — a journal entry, design doc, or post

If you're short on time, **cut Drill first and Build last.** Projects are the
spine; reading without building doesn't stick.

Steps are numbered in dependency order — each assumes the ones above it. Nothing
here is dated. Go at whatever pace you actually go at.

### One ordering decision worth knowing

The Book comes before DDIA even though `LEAD_AI_CURRICULUM.md` starts both
together. The eval runner is the spine artifact and it's written in Rust, so the
language has to be in your hands first. DDIA picks up at step 14, once Rust
shifts from reading to drilling.

If you want a parallel track rather than a strict line, DDIA (steps 14–15, 18,
24) is the one thing here that can run alongside anything above it without
breaking a dependency.

---

## Already done

- [x] Rust toolchain installed (`rustc` / `cargo` 1.97.1)
- [x] Cargo workspace at repo root; rust-analyzer resolving all crates
- [x] Book ch. 1 — `projects/hello_cargo` builds and runs

---

# Part I — Rust fundamentals

*Goal: you write Rust without fighting the borrow checker daily.*

### 1 · Setup and finish the guessing game

- [ ] Install Rustlings: `cargo install rustlings` then `rustlings init`
- [ ] **Read** Book ch. 1–3 *(skim — you know this; read for syntax only)*
- [ ] **Build** finish `projects/guessing_game` per Book ch. 2 — it currently
      reads stdin but has no secret number: add the `rand` dependency, generate
      the number, `match` on `cmp::Ordering`, loop until correct, handle
      unparseable input
- [ ] **Drill** Rustlings `intro`, `variables`, `functions`, `if`, `primitive_types`
- [ ] **Write** what surprised you about `Cargo.toml`, `match`, and shadowing

### 2 · Ownership

*The single most important step in Part I. Don't rush it.*

- [ ] **Read** Book **ch. 4 — Understanding Ownership** *(slow — read it twice)*
- [ ] **Drill** Rustlings `move_semantics`, `vecs`
- [ ] **Build** start Project 1 (Compound Interest CLI), milestones 1–2
      ```bash
      cargo new projects/interest_calculator
      ```
- [ ] **Write** explain move vs. borrow vs. copy in your own words, no jargon.
      If you can't, re-read ch. 4 before moving on.

### 3 · Structs, enums, pattern matching

- [ ] **Read** Book ch. 5–6 — structs, enums, `match`, `Option`
- [ ] **Drill** Rustlings `structs`, `enums`, `options`
- [ ] **Build** Project 1, milestone 3 — model `Compounding` as an `enum`, read
      `std::env::args()`, parse to numbers
- [ ] **Write** where `Option` replaces something you'd have done with `None`/`NA`

### 4 · Modules and collections

- [ ] **Read** Book ch. 7–8 — modules, `Vec`, `String`, `HashMap`
- [ ] **Drill** Rustlings `strings`, `modules`, `hashmaps`
- [ ] **Build** Project 1, milestone 4 — split into modules; handle negative
      rates and unparseable input without panicking
- [ ] **Write** `&str` vs. `String`, in your own words

### 5 · Error handling → Project 1 done

- [ ] **Read** Book **ch. 9 — Error handling** *(slow — the `try/except` mindset shift)*
- [ ] **Drill** Rustlings `error_handling`
- [ ] ✅ **Project 1 done** — amortization table checked against a spreadsheet
      you build yourself; bad input gives a friendly message, not a backtrace
- [ ] **Write** your own rule of thumb for `panic!` vs. `Result`

### 6 · Generics, traits, lifetimes

- [ ] **Read** Book **ch. 10 — Generics, Traits, Lifetimes** *(slow — read twice)*
- [ ] **Drill** Rustlings `generics`, `traits`, `lifetimes`
- [ ] **Build** start Project 2 (CSV Stats Summarizer), milestones 1–2
      ```bash
      cargo new projects/csv_stats
      ```
- [ ] **Write** traits vs. Python `ABC`/protocols vs. R S4 — where the analogy breaks

### 7 · Tests and your first real program

- [ ] **Read** Book ch. 11 (tests) and skim ch. 14 (Cargo) — reference from here on
- [ ] **Build** Book ch. 12 — **build minigrep, don't just read it**
- [ ] **Build** Project 2, milestone 3 — all columns via `HashMap<String, ColumnStats>`

### 8 · Iterators and closures

- [ ] **Read** Book ch. 13 — iterators & closures *(a nicer version of Python generators)*
- [ ] **Drill** Rustlings `iterators`
- [ ] **Build** Project 2, milestone 4 — skip malformed rows, report how many

### 9 · Project 2 done

- [ ] ✅ **Project 2 done** — milestone 5: refactor to the `csv` crate; stats
      match pandas `describe()` on a messy real file
- [ ] **Drill** start [100 Exercises to Learn Rust](https://rust-exercises.com/100-exercises/)
- [ ] **Write** the `csv`-crate refactor diff — what did the crate buy you?

### 10 · Smart pointers

- [ ] **Read** Book **ch. 15 — `Box`, `Rc`, `RefCell`** *(slow — the memory model gets concrete)*
- [ ] **Drill** Rustlings `smart_pointers`
- [ ] **Write** stack vs. heap; `Box` vs. `Rc` vs. `RefCell` — when each

### 11 · Concurrency

- [ ] **Read** Book ch. 16 — fearless concurrency
- [ ] **Skim** Book ch. 17 — async *(a real read comes at step 19)*
- [ ] **Drill** Rustlings `threads`

### 12 · Finish the drills

- [ ] **Skim** Book ch. 18–20 — OOP patterns, advanced *(reference only)*
- [ ] ✅ **Rustlings complete** — all exercises
- [ ] ✅ **100 Exercises complete**

### 13 · Checkpoint — Rust fundamentals

- [ ] I can explain *why* a value moved before the compiler tells me
- [ ] I reach for the right container (`Vec`/`HashMap`/`Box`/`Rc`) without thinking
- [ ] I model errors with a custom `enum` + `Result`, not panics
- [ ] I can write a CLI that reads a file, parses it, and handles errors cleanly

---

# Part II — Distributed systems and the first real build

*Goal: eval runner v1 grades your nine existing specs end to end.*

### 14 · DDIA, first half

- [ ] **Read** DDIA ch. 1 — reliable, scalable, maintainable applications
- [ ] **Read** DDIA ch. 2 — data models and query languages
- [ ] **Read** DDIA ch. 3 — storage and retrieval
- [ ] **Read** DDIA ch. 4 — encoding and evolution
- [ ] **Write** notes per chapter — this book rewards note-taking more than most

### 15 · DDIA, distributed data

- [ ] **Read** DDIA ch. 5 — replication
- [ ] **Read** DDIA ch. 6 — partitioning
- [ ] **Read** DDIA ch. 7 — transactions
- [ ] **Write** what "eventual" actually costs you, with a real example

### 16 · MIT 6.5840 opens

- [ ] **Course** lectures 1–3
- [ ] **Course** ✅ **Lab 1 (MapReduce)** *(Go, so it's pure systems learning)*

### 17 · Eval runner v1

The runner your `agentic_engineering` repo explicitly lacks. Sequential and
single-harness is fine — correctness before complexity.

- [ ] **Build** design sketch — discovery + scoring model. One page, no code.
- [ ] **Build** discover `agents/*/eval/*.eval.yaml`, parse them, print what it found
- [ ] **Build** drive one agent through one harness
- [ ] **Build** score the `expect` assertions from the event stream
- [ ] ✅ **Eval runner v1 done** — prints a report, grades all nine specs
- [ ] **Write** one post: what building v1 taught you

### 18 · Consensus

- [ ] **Read** DDIA ch. 8 — the trouble with distributed systems
- [ ] **Read** DDIA ch. 9 — consistency and consensus
- [ ] **Course** ✅ **Lab 2 (Raft)** — the hardest lab; give it real time

### 19 · Checkpoint — systems

- [ ] I can explain linearizability vs. eventual consistency with a real example
- [ ] My Raft lab passes its tests

---

# Part III — Async, production Rust, and evals

*Goal: a prompt or model change triggers CI that quantifies the regression, and
you can defend the statistics behind the threshold.*

### 20 · Async for real

- [ ] **Read** Book **ch. 17 — async/await**, properly this time
- [ ] **Build** Project 6 (Async Market-Data Fetcher), milestones 1–3 — a `tokio`
      drill before you wire concurrency into the runner
- [ ] ✅ **Project 6 done** — N tickers concurrently beats sequential, and one
      failing ticker doesn't kill the run

### 21 · Production discipline

- [ ] **Read** *Zero To Production in Rust* (Palmieri) — testing, telemetry, CI, deployment
- [ ] **Reference** *Programming Rust* ch. 4–5 (ownership, references) and ch. 11
      (traits and generics) — reach for these when Z2P leaves you wanting the deeper "why"

### 22 · The LLM systems map

- [ ] **Read** *AI Engineering* (Huyen) — evals, RAG, fine-tuning vs. prompting, inference
- [ ] **Course** *AI Evals for Engineers* (Husain & Shankar) — error analysis and eval design
- [ ] **Write** one post: error analysis on your own eval suite

### 23 · Reliability

- [ ] **Read** SRE book — SLOs, monitoring, release engineering *(skim the rest)*
- [ ] **Read** DDIA ch. 10–11 — batch and stream processing
- [ ] ✅ **DDIA ch. 12 + wrap-up — book done**
- [ ] **Course** ✅ **6.5840 Lab 3**

### 24 · Eval runner v2

- [ ] **Build** parallel execution — you know Rust's async story now
- [ ] **Build** job log and reconciliation
- [ ] **Build** JUnit/JSON output so it can gate CI
- [ ] **Build** regression detection — store baseline scores, fail on a drop
- [ ] **Build** wire it into the repo's GitHub Actions
- [ ] ✅ **Eval runner v2 done**
- [ ] **Build** *(stretch)* statistical treatment of flaky evals — when is a
      pass-rate drop significant? You are unusually qualified to answer this.

### 25 · The economics memo

- [ ] **Write** one page, decision-first: latency/cost/quality across model tiers,
      caching on and off, with your own eval suite as the quality metric

### 26 · Checkpoint — evals

- [ ] A model or prompt change in my repo triggers CI that quantifies regression
- [ ] I can defend the statistics behind the pass/fail threshold
- [ ] I've written `async` code and know when to use it vs. threads

---

# Part IV — Model risk, governance, and professional Rust

*Goal: you could sit across from your firm's model risk team and negotiate what
"validated" means for an LLM system — in their language.*

### 27 · The constitution

- [ ] **Read** **SR 11-7** — the Fed's model risk guidance, ~20 pages. The primary
      source, not a summary.

### 28 · Professional Rust, foundations

- [ ] **Read** *Rust for Rustaceans* ch. 1 — Foundations *(ownership and variance, with rigor)*
- [ ] **Read** *Rust for Rustaceans* ch. 2 — Types *(memory layout, alignment, coherence)*
- [ ] **Watch** two or three *Crust of Rust* episodes (Gjengset) alongside it

### 29 · Red-team your own agents, part 1

- [ ] **Build** prompt injection via tool results
- [ ] **Write** ADRs, retroactively, for the significant choices in the eval runner

### 30 · Frameworks and attack surface

- [ ] **Read** NIST AI RMF 1.0 + the Generative AI profile
- [ ] **Read** OWASP Top 10 for LLM Applications
- [ ] **Read** Simon Willison's prompt-injection series — the attacker's-eye view

### 31 · Professional Rust, interfaces

You're designing the runner's plugin surface now — the right moment for these.

- [ ] **Read** *Rust for Rustaceans* ch. 3 — Designing Interfaces
- [ ] **Read** *Rust for Rustaceans* ch. 4 — Error Handling *(libraries vs. applications)*
- [ ] **Read** *(as needed)* ch. 5–8 — project structure, testing, macros, async

### 32 · Red-team your own agents, part 2

- [ ] **Build** data exfiltration attempts through the `warehouse-server` connection
- [ ] **Build** jailbreaks of the agents' guardrails
- [ ] ✅ **Red-team evals done** — the runner now does security regression testing
- [ ] **Write** one post: security regression testing for agents

### 33 · Regulation and idiom

- [ ] **Read** EU AI Act — risk tiers and what "high-risk" obligates *(a good summary is fine)*
- [ ] **Read** *Effective Rust* — work through the 35 items

### 34 · Model validation pack

- [ ] **Write** ✅ model card + SR 11-7-style validation doc for one real system:
      purpose, assumptions, limitations, monitoring plan, effective challenge
- [ ] **Build** encode the pattern as `assess-model-risk` and `red-team-agent` skills

### 35 · Checkpoint — governance

- [ ] I can design an API around a trait and swap implementations without touching callers
- [ ] I could negotiate "validated" with an MRM team, in their language
- [ ] I can read std docs and understand the trait bounds on a signature

---

# Part V — Domain crates and staff-level craft

*Goal: a design doc you wrote changed a real decision at work.*

### 36 · The staff genre

- [ ] **Read** *The Staff Engineer's Path* (Reilly)
- [ ] **Write** ✅ **design doc / RFC at work #1** — context, options, decision,
      risks. Circulate it. Absorb the feedback publicly.

### 37 · Data crates

- [ ] **Learn** Polars (Rust core) — basics
- [ ] **Learn** ndarray — basics

### 38 · The capstone

Pick **one**. Both are good; you don't need both.

- [ ] **Build** choose: **Project 5** (Monte Carlo option pricer) *or*
      **Project 8** (mini backtesting engine)
- [ ] **Build** milestones 1–2 *(see `PROJECTS.md`)*
- [ ] **Build** milestones 3–4
- [ ] ✅ **Capstone done** — meets its definition of done

### 39 · Archetypes and inference

- [ ] **Read** *Staff Engineer* (Larson) — the archetypes and the politics
- [ ] **Learn** Candle **or** Burn — one model, running inference
- [ ] **Write** ✅ **design doc #2**

### 40 · Build vs. buy

- [ ] **Write** build-vs-buy analysis for one AI capability — total cost,
      lock-in, exit path, presented decision-first
- [ ] **Write** ✅ **design doc #3**
- [ ] **Write** *(if your firm does them)* volunteer for an incident write-up

### 41 · Close the loop

- [ ] **Build** encode `write-design-doc` and `build-vs-buy` as skills — the
      discipline you practiced becomes infrastructure you reuse
- [ ] **Learn** *(optional)* Linfa — one classical ML model

### 42 · Final checkpoint

- [ ] Stack vs. heap is intuitive; I know when a `clone` is cheap vs. expensive
- [ ] I've written concurrent code and understand `Send`/`Sync`
- [ ] The borrow checker has become mostly invisible — I fight it rarely
- [ ] A design doc I wrote changed a real decision
- [ ] I know which staff archetype I'm growing toward

---

# Running alongside everything

Not sequenced — these are habits, not steps.

- [ ] **One writing output per month** — post, design-doc section, or journal
      entry. Teaching is the fastest consolidation, and visibility is half the lead role.
- [ ] **Feed set up:** Simon Willison, Chip Huyen, Hamel Husain, Eugene Yan, Will Larson

---

# Off the main line

Deliberately not sequenced above. Do them if you have genuine slack; skip
without guilt.

- [ ] **Project 3** — Portfolio Ledger ★★★☆☆
- [ ] **Project 4** — kNN classifier from scratch ★★★☆☆
- [ ] **Project 7** — PyO3 hot path ★★★★☆ *(the highest-leverage elective — the
      pattern you'd actually use at work. Slots naturally after step 34.)*
- [ ] **Book ch. 21** — multithreaded web server
- [ ] *Programming Rust* — ch. 8–9, 13, 15, 19 beyond the priority chapters
- [ ] *Rust in Action* — backfills computer-architecture intuition
- [ ] *Comprehensive Rust* (Google) — alternative to 100 Exercises, not an addition

**Why Projects 3 and 4 are here:** the eval runner already forces modules, custom
error types, `serde`, and trait design — the same lessons — and it's the artifact
that ties everything together.

---

# Stumbling blocks

_Running log: the errors that confused you, the "aha" moments, and links worth
revisiting. Future-you will thank you._

-
