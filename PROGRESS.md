# Progress Tracker

A checklist for the [`CURRICULUM.md`](./CURRICULUM.md) reading, the
[`PROJECTS.md`](./PROJECTS.md) build ladder, and the 12-month
[`LEAD_AI_CURRICULUM.md`](./LEAD_AI_CURRICULUM.md) track. Tick boxes as you go
(`[ ]` → `[x]`). Started: _(fill in)_.

---

## Reading

### Phase 0 — Setup & orientation
- [ ] Toolchain installed (`rustup`, `cargo`, `rust-analyzer`)
- [ ] Skimmed Comprehensive Rust — Day 1

### Phase 1 — The Book (`https://doc.rust-lang.org/book/`)
- [ ] Ch. 1–3 — setup, guessing game, common concepts *(skim)*
- [ ] **Ch. 4 — Ownership** *(slow, read twice)*
- [ ] Ch. 5–6 — structs, enums, pattern matching
- [ ] Ch. 7–8 — modules, collections
- [ ] **Ch. 9 — Error handling** *(slow)*
- [ ] **Ch. 10 — Generics, Traits, Lifetimes** *(slow, read twice)*
- [ ] Ch. 11 — writing tests
- [ ] Ch. 12 — CLI project (minigrep) *(build it)*
- [ ] Ch. 13 — iterators & closures
- [ ] Ch. 14 — Cargo & crates.io *(skim)*
- [ ] **Ch. 15 — Smart pointers** *(slow)*
- [ ] Ch. 16 — fearless concurrency
- [ ] Ch. 17 — async/await *(skim first pass)*
- [ ] Ch. 18–20 — OOP, patterns, advanced *(skim)*
- [ ] Ch. 21 — multithreaded web server *(optional capstone)*
- [ ] **Rustlings — completed all exercises** *(in parallel)*

### Phase 2 — Reinforcement drills
- [ ] 100 Exercises to Learn Rust — completed *(recommended primary)*
- [ ] (or) Comprehensive Rust — completed

### Phase 3 — *Programming Rust*, 2nd ed. (the "why")
- [ ] Ch. 4 — Ownership and Moves
- [ ] Ch. 5 — References
- [ ] Ch. 8–9 — Crates/Modules, Structs
- [ ] Ch. 11 — Traits and Generics
- [ ] Ch. 13 — Utility Traits
- [ ] Ch. 15 — Iterators
- [ ] Ch. 19 — Concurrency

### Phase 4 — Idiomatic / professional
- [ ] Rust for Rustaceans — ch. 1–4 (Foundations, Types, Interfaces, Errors)
- [ ] Rust for Rustaceans — ch. 5–8 (as needed)
- [ ] Watched some *Crust of Rust* (Jon Gjengset) videos
- [ ] Effective Rust — read through the items

### Phase 5 — Domain (start anytime from Phase 2)
- [ ] Polars (Rust core) — basics
- [ ] ndarray — basics
- [ ] Candle **or** Burn — a first model/inference
- [ ] Linfa — a classical ML model
- [ ] PyO3 / maturin — a Rust module callable from Python
- [ ] (optional) Rust in Action — a couple of projects

---

## Projects (see [`PROJECTS.md`](./PROJECTS.md))

- [ ] **1 — Compound Interest & Amortization CLI** ★☆☆☆☆ *(Phase 1)*
- [ ] **2 — CSV Stats Summarizer** ★★☆☆☆ *(Phase 1–2)*
- [ ] **3 — Portfolio Ledger** ★★★☆☆ *(Phase 2)*
- [ ] **4 — kNN Classifier from scratch** ★★★☆☆ *(Phase 3)*
- [ ] **5 — Monte Carlo Option Pricer (parallel)** ★★★★☆ *(Phase 3)*
- [ ] **6 — Async Market-Data Fetcher** ★★★★☆ *(Phase 4)*
- [ ] **7 — Rust hot-path as a Python module (PyO3)** ★★★★☆ *(Phase 4–5)*
- [ ] **8 — Capstone: Mini Backtesting Engine** ★★★★★ *(Phase 5)*

*(Per the [12-month map](./LEAD_AI_CURRICULUM.md): 1–2 are warm-ups, 6 is the
async drill, 5 → 7 are the work-relevant electives, 3/4/8 are optional.)*

---

## Lead AI Engineer track (see [`LEAD_AI_CURRICULUM.md`](./LEAD_AI_CURRICULUM.md))

### Phase 1 — Months 1–3 · Production & distributed systems
- [ ] *Designing Data-Intensive Applications* — read cover to cover (~1 ch/week)
- [ ] MIT 6.5840 — lectures
- [ ] MIT 6.5840 — Lab 1 (MapReduce)
- [ ] MIT 6.5840 — Lab 2 (Raft)
- [ ] MIT 6.5840 — Lab 3
- [ ] *Zero To Production in Rust* — started
- [ ] **Eval runner v1** — grades all nine existing specs end-to-end
- [ ] Self-check: can explain linearizability vs. eventual consistency with a real example

### Phase 2 — Months 4–6 · LLM systems, evals & reliability
- [ ] *AI Engineering* (Huyen) — read
- [ ] *AI Evals for Engineers* (Husain & Shankar) — completed
- [ ] SRE book — SLOs, monitoring, release engineering chapters
- [ ] *Zero To Production in Rust* — finished
- [ ] **Eval runner v2** — parallel, job log, CI gate, regression detection
- [ ] Wired into GitHub Actions; a prompt/model change triggers quantified regression
- [ ] Stretch: statistical treatment of flaky evals (significance of a pass-rate drop)
- [ ] Inference-economics memo — one page, decision-first

### Phase 3 — Months 7–9 · Model risk, governance & security
- [ ] SR 11-7 — read the primary source
- [ ] NIST AI RMF 1.0 + Generative AI profile — read
- [ ] EU AI Act — risk tiers and high-risk obligations understood
- [ ] OWASP Top 10 for LLM Applications + Willison's prompt-injection series
- [ ] **Red-team evals** in the repo (injection, exfiltration, jailbreaks)
- [ ] **Model validation pack** — model card + SR 11-7-style validation doc
- [ ] Encoded as `assess-model-risk` and `red-team-agent` skills
- [ ] Self-check: could negotiate "validated" with the MRM team, in their language

### Phase 4 — Months 10–12 · Staff-level leadership craft
- [ ] *The Staff Engineer's Path* (Reilly) — read
- [ ] *Staff Engineer* (Larson) — read
- [ ] Design doc/RFC at work — month 10
- [ ] Design doc/RFC at work — month 11
- [ ] Design doc/RFC at work — month 12
- [ ] Build-vs-buy analysis — presented decision-first
- [ ] ADRs written retroactively for own repos
- [ ] (if available) Incident review written up
- [ ] `write-design-doc` and `build-vs-buy` skills encoded
- [ ] Self-check: a design doc I wrote changed a real decision; I know my archetype direction

### Continuous threads (all year)
- [ ] Writing: one post per month *(log them in Notes below)*
- [ ] Feed set up: Willison, Huyen, Husain, Yan, Larson

---

## Skill self-checks

Not "did I read it" but "can I do it." Tick these honestly — they're the real
measure of progress.

- [ ] I can explain *why* a value moved, and predict when the borrow checker will complain — before I hit compile.
- [ ] I reach for the right container (`Vec`/`HashMap`/`Box`/`Rc`/`RefCell`) without thinking.
- [ ] I model errors with a custom `enum` + `Result` instead of reaching for panics.
- [ ] I can design an API around a trait and swap implementations without touching callers.
- [ ] Stack vs. heap is intuitive; I know when a `clone` is cheap vs. expensive.
- [ ] I can read std-library docs and understand the trait bounds on a signature.
- [ ] I've written concurrent code (threads/`rayon`) and understand `Send`/`Sync`.
- [ ] I've written `async` code and know when to use it vs. threads.
- [ ] The borrow checker has become mostly invisible — I fight it rarely.

---

## Notes & stumbling blocks

_Keep a running log here — the errors that confused you, the "aha" moments, and
links worth revisiting. Future-you will thank you._

-
