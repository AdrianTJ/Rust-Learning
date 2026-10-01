# The Checklist

The one file. Every track, every step, in order. Tick boxes here and nowhere else.

## Where I am

| Track | Pace | Next step | Folder |
|---|---|---|---|
| **M · ML systems** | active, most of the hours | M0 | [`ml-systems/`](./ml-systems/) |
| **R · Rust** | slow, a few hours a week | R1 | [`rust/`](./rust/) |
| **T · TypeScript** | parked | T0 | [`typescript/`](./typescript/) |

*Update "Next step" when you finish one. The tracks don't depend on each other;
work on them in any order.*

**Line types:** **Read** a book or docs · **Build** code · **Drill** short
graded exercises · **Train** a training run you watch and explain · **Write** a
note in [`Notes/`](./Notes/). Short on time? Cut **Drill** first, **Write**
second, **Build**/**Train** never.

**Why each step is there:** [`docs/`](./docs/). You shouldn't need it to know
what's next.

---

# M · ML systems

*Train models with your own hands, then design the systems around them.
Build a framework in NumPy → train a GPT in PyTorch → serve it behind an API →
go deep on distributed systems.*

**Started:** _______ · **Why:** [`docs/ml-systems-overview.md`](./docs/ml-systems-overview.md),
[`docs/ml-systems-curriculum.md`](./docs/ml-systems-curriculum.md)

### M0 · Setup

- [ ] Python ≥ 3.10; install TinyTorch into `ml-systems/` per
      [`ml-systems/README.md`](./ml-systems/README.md); `tito setup` passes
- [ ] TrenTorch web account at trentorch.com *(this is the Drill line)*
- [ ] GPU access for later: Colab or Kaggle is enough to start

## Part I · Build the framework (TinyTorch 01–08)

### M1 · Tensors to layers

- [ ] **Build** TinyTorch 01 Tensor, 02 Activations, 03 Layers, 04 Losses *(go fast)*
- [ ] **Drill** TrenTorch linear-algebra questions
- [ ] ✅ **Milestone: 1958 Perceptron**

### M2 · Data

- [ ] **Build** TinyTorch 05 Dataloader
- [ ] **Write** why batching exists, beyond "the GPU likes it"

### M3 · Autograd *(the important one; go slow)*

- [ ] **Build** TinyTorch 06 Autograd
- [ ] **Read** the *ML Systems* textbook section module 06 points to
- [ ] **Drill** TrenTorch backprop questions
- [ ] **Write** draw the graph for a two-layer MLP + cross-entropy; mark which
      tensors are saved for backward, and why

### M4 · Optimizers and the loop

- [ ] **Build** TinyTorch 07 Optimizers (SGD, momentum, Adam), 08 Training
- [ ] ✅ **Milestones: 1969 XOR, 1986 MLP**
- [ ] **Write** what Adam stores per parameter, and what that costs at 1B parameters

**Checkpoint:** I can explain `loss.backward()` step by step, and I know where
optimizer state lives.

## Part II · Train for real (PyTorch)

### M5 · Translate

- [ ] **Read** PyTorch *Learn the Basics* tutorial
- [ ] **Build** rebuild your MLP in PyTorch; name the TinyTorch module each piece replaces
- [ ] **Train** same data in both frameworks; the curves should match

### M6 · The procedure

- [ ] **Read** Karpathy, *A Recipe for Training Neural Networks*
- [ ] **Train** overfit one batch → check initial loss vs. `log(num_classes)` →
      LR range test → train properly
- [ ] **Train** break it on purpose (LR ×100, no `zero_grad`, shuffled labels);
      record what each failure looks like on the curve
- [ ] **Write** your own one-page training checklist

### M7 · Language models, by hand first

- [ ] **Build** TinyTorch 09 Convolutions *(✅ Milestone: 1998 CNN)*
- [ ] **Build** TinyTorch 10 Tokenization, 11 Embeddings, 12 Attention, 13 Transformers
- [ ] **Drill** TrenTorch transformer questions
- [ ] ✅ **Milestone: 2017 Transformer**

### M8 · makemore

- [ ] **Build** *Zero to Hero*, makemore lectures *(skip micrograd; you built it in M3)*
- [ ] **Train** a character-level model to the lecture's validation loss

### M9 · Reproduce GPT-2 (124M) *(the core of the track)*

- [ ] **Build** *Zero to Hero*, "Let's build GPT" + "Let's reproduce GPT-2",
      against `karpathy/build-nanogpt`
- [ ] **Train** pick the target validation loss *before* the run; hit it
- [ ] **Train** mixed precision, grad accumulation, warmup + cosine LR, grad
      clipping; turn each on separately and measure what it buys
- [ ] **Train** kill the run halfway, resume from checkpoint; no jump in the curve
- [ ] **Train** experiment tracking on every run from here on
- [ ] ✅ **GPT-2 reproduced**: target hit, every spike explained
- [ ] **Write** what training taught you that the theory didn't

**Checkpoint:** I can diagnose a stalled loss curve without guessing, and I know
where the memory goes: params, grads, optimizer state, activations.

## Part III · Serve it (systems design & APIs)

### M10 · Vocabulary

- [ ] **Read** Xu, *System Design Interview Vol. 1*: scale from zero to
      millions, back-of-the-envelope estimation, the design framework
- [ ] **Write** a capacity estimate for serving your GPT to 1,000 users

### M11 · The API

- [ ] **Build** FastAPI service around your model: typed request/response
      schemas, explicit errors, versioned path (`/v1/...`)
- [ ] **Build** a health check that actually runs the model
- [ ] **Build** per-request timeouts that stop GPU work
- [ ] **Write** what a `/v2` would change, and how `/v1` clients survive it

### M12 · Make it a service

- [ ] **Read** Xu: rate limiter; consistent hashing
- [ ] **Build** token-bucket rate limiting, per client
- [ ] **Build** idempotency keys on endpoints that do expensive work
- [ ] **Build** structured logs with request IDs; p50/p95/p99 latency
- [ ] **Build** load test; find the knee in the latency curve

### M13 · Batching and inference optimization

- [ ] **Build** dynamic batching; measure the latency/throughput tradeoff
- [ ] **Build** TinyTorch 14 Profiling → 19 Benchmarking *(✅ Milestone: MLPerf)*
- [ ] **Build** apply one technique from 15–18 to your service; re-run the load test
- [ ] **Write** the before/after numbers, and why they moved

### M14 · ML systems design

- [ ] **Read** Huyen, *Designing Machine Learning Systems*: deployment &
      prediction service; distribution shifts & monitoring
- [ ] **Build** one drift signal on your service's inputs

### M15 · Capstone

- [ ] **Build** TinyTorch 20 Capstone
- [ ] **Write** ✅ **design doc**: requirements, API contract, capacity estimate,
      batching/caching decisions with load-test evidence, failure modes, 100× traffic
- [ ] ✅ **Service done**: callable from the docs alone; degrades gracefully at 10× load

**Checkpoint:** I can whiteboard a model-serving system and defend each box, and
explain idempotency, backpressure, and p99 with my own examples.

## Part IV · Distributed systems (DDIA + MIT 6.5840)

*The layer beneath Part III: how the databases and services you'd sit on behave
when machines fail. The reading can start early. If Part II's training runs
leave you waiting on a GPU, read DDIA then.*

*Use DDIA's **2nd edition** (Kleppmann & Riccomini, 2026). Chapters are named by
topic below because its numbering differs from the 1st edition's.*

### M16 · DDIA, foundations

- [ ] **Read** trade-offs in data systems architecture; nonfunctional requirements
- [ ] **Read** data models and query languages; storage and retrieval
- [ ] **Read** encoding and evolution *(this is API versioning, for data)*
- [ ] **Write** notes per chapter

### M17 · DDIA, distributed data

- [ ] **Read** replication; sharding; transactions
- [ ] **Write** what "eventual" actually costs you, with a real example

### M18 · MIT 6.5840 opens

- [ ] **Read** *(if Go is new to you)* *A Tour of Go*, a weekend. The labs are in Go.
- [ ] **Course** lectures 1–3
- [ ] **Course** ✅ **Lab 1: MapReduce**
- [ ] **Course** ✅ **Lab 2: key/value server.** Retries and `ErrMaybe` are the
      idempotency problem from M12, made rigorous

### M19 · Consensus

- [ ] **Read** the trouble with distributed systems; consistency and consensus
- [ ] **Course** ✅ **Lab 3: Raft** *(the hardest lab; give it real time)*

### M20 · Finish

- [ ] **Read** batch processing; stream processing; the closing chapter(s) ✅ **DDIA done**
- [ ] **Course** ✅ **Lab 4: fault-tolerant key/value service** on your Raft

**Checkpoint:** I can explain linearizability vs. eventual consistency with a
real example, and my Raft passes its tests.

## M · Off the main line

- [ ] **Train** on your own domain data (market data, a work dataset), with an
      honest out-of-sample eval
- [ ] **Read** HF *Ultra-Scale Playbook*; **Train** your GPT on multiple GPUs (DDP, then FSDP)
- [ ] **Read** Stas Bekman, *ML Engineering Open Book*, as reference
- [ ] ✅ TinyTorch **Milestone: 2024 Custom Kernels**
- [ ] Google AIP (<https://google.aip.dev/>), as reference for API shape

---

# R · Rust

*The language, slowly, with the eval runner as the spine project and the Lead AI
reading (evals, model risk, staff craft) alongside it.*

**Set up:** 2026-07-25 · **Started:** _______ · **Why:**
[`docs/rust-curriculum.md`](./docs/rust-curriculum.md) (the reading),
[`docs/rust-projects.md`](./docs/rust-projects.md) (project milestones; "Project 2,
milestones 3–4" means look there), [`docs/lead-ai-year.md`](./docs/lead-ai-year.md)
(the non-Rust reading)

All `cargo` commands run from [`rust/`](./rust/).

**Habits, not steps:** one piece of writing a month (post, design-doc section,
journal entry); a feed of Simon Willison, Chip Huyen, Hamel Husain, Eugene Yan,
Will Larson.

### Already done

- [x] Rust toolchain installed (`rustc` / `cargo` 1.97)
- [x] Cargo workspace in `rust/`; rust-analyzer resolving all crates
- [x] Book ch. 1: `projects/hello_cargo` builds and runs

## Part I · Fundamentals

*Goal: you write Rust without fighting the borrow checker daily.*

### R1 · Setup and the guessing game

- [ ] Install Rustlings: `cargo install rustlings` then `rustlings init`
- [ ] **Read** Book ch. 1–3 *(skim; read for syntax)*
- [ ] **Build** finish `projects/guessing_game` per Book ch. 2: add `rand`,
      generate the number, `match` on `cmp::Ordering`, loop, handle bad input
- [ ] **Drill** Rustlings `intro`, `variables`, `functions`, `if`, `primitive_types`
- [ ] **Write** what surprised you about `Cargo.toml`, `match`, and shadowing

### R2 · Ownership *(the important one; go slow)*

- [ ] **Read** Book **ch. 4** *(read it twice)*
- [ ] **Drill** Rustlings `move_semantics`, `vecs`
- [ ] **Build** Project 1 (Compound Interest CLI), milestones 1–2:
      `cargo new projects/interest_calculator`
- [ ] **Write** move vs. borrow vs. copy, no jargon. Can't? Re-read ch. 4.

### R3 · Structs, enums, pattern matching

- [ ] **Read** Book ch. 5–6
- [ ] **Drill** Rustlings `structs`, `enums`, `options`
- [ ] **Build** Project 1, milestone 3
- [ ] **Write** where `Option` replaces your `None`/`NA` habits

### R4 · Modules and collections

- [ ] **Read** Book ch. 7–8
- [ ] **Drill** Rustlings `strings`, `modules`, `hashmaps`
- [ ] **Build** Project 1, milestone 4
- [ ] **Write** `&str` vs. `String`

### R5 · Error handling

- [ ] **Read** Book **ch. 9** *(slow; the `try/except` mindset shift)*
- [ ] **Drill** Rustlings `error_handling`
- [ ] ✅ **Project 1 done**: checked against your own spreadsheet; bad input is friendly
- [ ] **Write** your rule of thumb for `panic!` vs. `Result`

### R6 · Generics, traits, lifetimes *(slow; read twice)*

- [ ] **Read** Book **ch. 10**
- [ ] **Drill** Rustlings `generics`, `traits`, `lifetimes`
- [ ] **Build** Project 2 (CSV Stats), milestones 1–2: `cargo new projects/csv_stats`
- [ ] **Write** traits vs. Python protocols vs. R S4: where the analogy breaks

### R7 · Tests and minigrep

- [ ] **Read** Book ch. 11; skim ch. 14
- [ ] **Build** Book ch. 12 minigrep *(build it, don't just read it)*
- [ ] **Build** Project 2, milestone 3

### R8 · Iterators and closures

- [ ] **Read** Book ch. 13
- [ ] **Drill** Rustlings `iterators`
- [ ] **Build** Project 2, milestone 4

### R9 · Project 2 done

- [ ] ✅ **Project 2 done**: `csv` crate refactor; stats match pandas `describe()`
- [ ] **Drill** start *100 Exercises to Learn Rust*
- [ ] **Write** what the `csv` crate bought you

### R10 · Smart pointers *(slow)*

- [ ] **Read** Book **ch. 15**
- [ ] **Drill** Rustlings `smart_pointers`
- [ ] **Write** stack vs. heap; `Box` vs. `Rc` vs. `RefCell`

### R11 · Concurrency

- [ ] **Read** Book ch. 16; skim ch. 17 *(async, properly at R14)*
- [ ] **Drill** Rustlings `threads`

### R12 · Finish the drills

- [ ] **Skim** Book ch. 18–20
- [ ] ✅ **Rustlings complete**
- [ ] ✅ **100 Exercises complete**

**Checkpoint:** I know why a value moved before the compiler tells me; I pick
the right container without thinking; errors are `enum` + `Result`, not panics.

## Part II · The eval runner, v1

*Goal: eval runner v1 grades your nine existing specs end to end.*

### R13 · Eval runner v1

- [ ] **Build** design sketch: discovery + scoring model. One page, no code.
- [ ] **Build** discover `agents/*/eval/*.eval.yaml`; parse; print what was found
- [ ] **Build** drive one agent through one harness
- [ ] **Build** score the `expect` assertions from the event stream
- [ ] ✅ **Eval runner v1 done**: report grades all nine specs
- [ ] **Write** what building v1 taught you

## Part III · Async, production Rust, evals

*Goal: a prompt or model change triggers CI that quantifies the regression.*

### R14 · Async for real

- [ ] **Read** Book **ch. 17**, properly
- [ ] **Build** Project 6 (Async Market-Data Fetcher), milestones 1–3
- [ ] ✅ **Project 6 done**: concurrent beats sequential; one failure doesn't kill the run

### R15 · Production discipline

- [ ] **Read** *Zero To Production in Rust*
- [ ] **Reference** *Programming Rust* ch. 4–5, 11 when you want the deeper "why"

### R16 · The LLM systems map

- [ ] **Read** Huyen, *AI Engineering*
- [ ] **Course** Husain & Shankar, *AI Evals for Engineers*
- [ ] **Write** error analysis on your own eval suite

### R17 · Reliability

- [ ] **Read** *Site Reliability Engineering*: SLOs, monitoring, release engineering

### R18 · Eval runner v2

- [ ] **Build** parallel execution
- [ ] **Build** job log and reconciliation
- [ ] **Build** JUnit/JSON output
- [ ] **Build** regression detection: baseline scores, fail on a drop
- [ ] **Build** wire into GitHub Actions
- [ ] ✅ **Eval runner v2 done**
- [ ] **Build** *(stretch)* when is a pass-rate drop significant? You're unusually qualified.

### R19 · The economics memo

- [ ] **Write** one page, decision-first: latency/cost/quality across model
      tiers, caching on/off, your eval suite as the quality metric

**Checkpoint:** a model or prompt change triggers CI that quantifies regression,
and I can defend the statistics behind the threshold.

## Part IV · Model risk, governance, professional Rust

*Goal: you could negotiate what "validated" means with a model risk team, in their language.*

### R20 · The constitution

- [ ] **Read** **SR 11-7**, the primary source (~20 pages)

### R21 · Professional Rust, foundations

- [ ] **Read** *Rust for Rustaceans* ch. 1–2
- [ ] **Watch** two or three *Crust of Rust* episodes

### R22 · Red-team your agents, part 1

- [ ] **Build** prompt injection via tool results
- [ ] **Write** ADRs, retroactively, for the eval runner's big choices

### R23 · Frameworks and attack surface

- [ ] **Read** NIST AI RMF 1.0 + Generative AI profile
- [ ] **Read** OWASP Top 10 for LLM Applications
- [ ] **Read** Simon Willison's prompt-injection series

### R24 · Professional Rust, interfaces

- [ ] **Read** *Rust for Rustaceans* ch. 3–4; ch. 5–8 as needed

### R25 · Red-team your agents, part 2

- [ ] **Build** exfiltration attempts through `warehouse-server`
- [ ] **Build** jailbreaks of the agents' guardrails
- [ ] ✅ **Red-team evals done**: the runner does security regression testing
- [ ] **Write** security regression testing for agents

### R26 · Regulation and idiom

- [ ] **Read** EU AI Act: risk tiers and "high-risk" obligations *(a summary is fine)*
- [ ] **Read** *Effective Rust*, all 35 items

### R27 · Model validation pack

- [ ] **Write** ✅ model card + SR 11-7-style validation doc for one real system
- [ ] **Build** encode it as `assess-model-risk` and `red-team-agent` skills

**Checkpoint:** I can design an API around a trait and swap implementations
without touching callers.

## Part V · Domain crates and staff craft

*Goal: a design doc you wrote changed a real decision at work.*

### R28 · The staff genre

- [ ] **Read** *The Staff Engineer's Path* (Reilly)
- [ ] **Write** ✅ **design doc #1 at work**. Circulate it.

### R29 · Data crates

- [ ] **Learn** Polars (Rust core) and ndarray basics

### R30 · Capstone *(pick one)*

- [ ] **Build** Project 5 (Monte Carlo pricer) *or* Project 8 (backtester), milestones 1–4
- [ ] ✅ **Capstone done**

### R31 · Archetypes and inference

- [ ] **Read** *Staff Engineer* (Larson)
- [ ] **Learn** Candle **or** Burn: one model, running inference
- [ ] **Write** ✅ **design doc #2**

### R32 · Build vs. buy

- [ ] **Write** build-vs-buy for one AI capability, decision-first
- [ ] **Write** ✅ **design doc #3**

### R33 · Close the loop

- [ ] **Build** `write-design-doc` and `build-vs-buy` skills

**Final checkpoint:** the borrow checker is mostly invisible; I understand
`Send`/`Sync`; a design doc I wrote changed a real decision.

## R · Off the main line

- [ ] Project 3 (Portfolio Ledger) · Project 4 (kNN from scratch)
- [ ] Project 7 (PyO3 hot path): the highest-leverage elective; fits after R27
- [ ] Book ch. 21 (multithreaded web server)
- [ ] *Rust in Action*: backfills computer-architecture intuition
- [ ] Linfa: one classical model

---

# T · TypeScript *(parked)*

*3–4 weeks, then stop. Ends in an agent harness that does one real task through
an MCP server. Not a prerequisite for anything else.*

**Started:** _______ · **Finished:** _______ · **Why:**
[`docs/typescript-overview.md`](./docs/typescript-overview.md),
[`docs/typescript-curriculum.md`](./docs/typescript-curriculum.md)

**Exit rule:** when T10 is done, TypeScript study stops. Unfinished bits become
reference.

### T0 · Setup

- [ ] `node --version` ≥ 22; scaffold per [`typescript/README.md`](./typescript/README.md)
- [ ] Anthropic API key in `typescript/harness/.env` *(gitignored)*

## Part I · The language, fast (~1 week)

### T1 · Runtime

- [ ] **Build** `src/index.ts` runs with `npx tsx src/index.ts`
- [ ] **Build** import one file from another; hit the `.js`-extension wall on purpose

### T2 · The language subset

- [ ] **Read** TS Handbook: *Everyday Types*, *Narrowing*, *Generics*. Stop there.
- [ ] **Build** agent state as a discriminated union with a `never` exhaustiveness check;
      delete a `case`, watch it fail to compile
- [ ] **Build** narrow an `unknown`, not an `any`

### T3 · Async

- [ ] **Build** `Promise.all` / `allSettled` / `race` over three fake tools
- [ ] **Build** `withTimeout(promise, ms)` from `Promise.race`

### T4 · Zod

- [ ] **Read** the Zod docs, all of them
- [ ] **Build** one schema → `z.infer` type + `z.toJSONSchema()`; add a field, all three move
- [ ] **Build** `safeParse` failures returned as text a model could act on

## Part II · The harness (~2 weeks)

### T5 · The loop

- [ ] **Read** Anthropic docs: messages, `tool_use` / `tool_result`
- [ ] **Build** `defineTool` generic over the Zod schema; two tools
- [ ] **Build** the loop: history, dispatch, validate, append, terminate
- [ ] **Build** the four unhappy paths, each appending a tool result
- [ ] ✅ **Loop done**: unhappy paths leave history valid

### T6 · Robustness *(the important one)*

- [ ] **Build** `AbortSignal` through the model call and every tool
- [ ] **Build** Ctrl-C cancels mid-tool, cleanly
- [ ] **Build** timeouts; retry with backoff + jitter, honouring `retry-after`
- [ ] ✅ **Robustness done**: you can leave it running unattended

### T7 · Streaming

- [ ] **Build** stream text; assemble `input_json_delta` into tool args
- [ ] **Build** cancellation still works mid-stream

### T8 · Safety

- [ ] **Build** risk classification + an approval hook before dangerous tools
- [ ] **Build** a `spawn`-based bash tool: streamed, timed out, process-group kill, truncated
- [ ] **Build** a file tool with a post-canonicalization path allowlist; fail to escape it
- [ ] ✅ **Safety done**: asks before `rm`; can't be talked out of its allowlist

### T9 · Testing

- [ ] **Build** model client behind an interface + a scripted fake
- [ ] **Build** `node --test` for happy path, validation failure, throw, retry,
      cancellation, max steps
- [ ] ✅ **Tests done**: no API key, no network

## Part III · Capstone

### T10 · Plug into the real ecosystem

- [ ] **Read** MCP spec overview + TypeScript SDK
- [ ] **Build** make your harness an MCP client; connect one off-the-shelf MCP server
- [ ] **Build** run one real task end to end (e.g. "find and fix the failing test in
      this repo"), with approval prompts and cancellation working
- [ ] **Read** Claude Code or Gemini CLI source: how does Ctrl-C reach a
      subprocess? What happens when context fills?
- [ ] ✅ **Harness done**. **Write** three things the real harnesses do that
      yours doesn't, and which you'd port to Rust first. **Stop here.**

---

# Stumbling blocks

_Errors that confused you, "aha" moments, links worth revisiting. Any track._

-
