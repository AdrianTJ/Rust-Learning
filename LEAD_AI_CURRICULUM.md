# AI Engineer Curriculum — 12 Months

The companion track to [`CURRICULUM.md`](./CURRICULUM.md) (Rust) and
[`PROJECTS.md`](./PROJECTS.md) (the build ladder). Built for: applied
mathematician → AI engineer.

Strengths already banked: statistical modeling, optimization, CLI-first data
work, agentic engineering, communication. Gaps this closes, in priority order:
**production/distributed systems, LLM eval & reliability engineering, model
risk & AI governance in finance, AI security, and staff-level leadership
craft.**

**Rust runs as a continuous thread, not a phase** — always aimed at the current
phase's project, never at puzzles. That's what "side by side" means here: the
Rust curriculum tells you *what to read about the language*; this file tells you
*what to point it at each quarter*.

---

## How the two curricula interleave

At ~6–8 hrs/week you cannot run two full curricula plus two project tracks.
So, one explicit decision up front:

> **The eval runner is the spine project of the year.** It's the artifact that
> ties all four phases together (v1 → concurrent v2 → security regression →
> the thing your design docs are about). The finance ladder in `PROJECTS.md`
> becomes the *gym*: warm-ups and electives scheduled where they teach a Rust
> concept the spine is about to need.

### The 12-month map

| Months | This track | Rust reading ([`CURRICULUM.md`](./CURRICULUM.md)) | Building |
|---|---|---|---|
| **1–3** | Production & distributed systems (DDIA, MIT 6.5840) | Phases 1–2: The Book + Rustlings, 100 Exercises | Warm-ups: Projects **1–2** (finance ladder) → **eval runner v1** |
| **4–6** | LLM systems, evals & reliability (Huyen, Husain/Shankar, SRE) | Phase 3: *Programming Rust* + *Zero To Production*; Book ch. 17 (async) for real now | Project **6** (async fetcher, as a tokio drill) → **eval runner v2** |
| **7–9** | Model risk, governance & security (SR 11-7, NIST, OWASP) | Phase 4: *Rust for Rustaceans*, *Effective Rust* | **Red-team evals** + **model validation pack**; elective: Project **7** (PyO3) |
| **10–12** | Staff-level leadership craft (Reilly, Larson) | Phase 5: domain crates (Polars, ndarray, Candle) | Design docs, ADRs, build-vs-buy; elective capstone: Project **5** or **8** |

Finance-ladder projects **3, 4, 8** are pure electives — do them if a month has
slack, skip without guilt. Projects **5 → 7** (Monte Carlo → PyO3) remain the
most work-relevant pair if you want a second artifact for the portfolio.

**Weekly rhythm (~6–8 hrs):** 2–3 hrs reading (one book as the "spine" at a
time), 3–4 hrs project work, 1 hr writing (notes, a design doc section, or a
blog post — you edit a journal; use that muscle, it compounds everything else).

---

## Phase 1 · Months 1–3 — Production & distributed systems foundations

The gap between "mathematician who codes well" and "lead engineer." Everything
else stacks on this.

**Spine book:** *Designing Data-Intensive Applications* — Kleppmann. Read it
cover to cover, ~1 chapter/week. The single highest-leverage book for you.

**Course:** [MIT 6.5840](https://pdos.csail.mit.edu/6.824/) (formerly 6.824),
Distributed Systems — lectures and labs are free online. Do at least Labs 1–3
(MapReduce, Raft). Labs are in Go, which you already know, so the learning is
pure systems, not syntax.

**Rust thread:** The Book to completion with Rustlings, per
[`CURRICULUM.md`](./CURRICULUM.md) Phases 1–2 (slow chapters: 4, 9, 10, 15).
Then start [*Zero To Production in Rust*](https://www.zero2prod.com/)
(Palmieri) — it teaches Rust *and* production discipline (testing, telemetry,
CI, deployment) in one pass, which is exactly your combination of gaps. It
slots between the Rust curriculum's Phase 2 and 3 and can partially replace
*Programming Rust* if time is tight — read *Programming Rust*'s ch. 4–5 and 11
regardless.

**Warm-up builds (weeks 1–6):** finance-ladder Projects **1** (interest CLI)
and **2** (CSV stats) — small enough to finish while still reading The Book,
and they teach exactly the `Result`/iterator/module skills the eval runner
needs on day one.

**Project — eval runner v1 (Rust):** the runner your `agentic_engineering`
repo explicitly lacks. Discover `agents/*/eval/*.eval.yaml`, drive one agent
through one harness, score the `expect` assertions from the event stream, print
a report. Sequential and single-harness is fine — correctness first (your own
`model-data` skill's rule: baseline before complexity).

**Done when:** you can explain the difference between linearizability and
eventual consistency with a real example; your Raft lab passes tests; eval
runner v1 grades your nine existing specs end-to-end.

---

## Phase 2 · Months 4–6 — LLM systems, evals & reliability

Where your math background becomes an unfair advantage: quantifying whether AI
systems actually work.

**Spine book:** *AI Engineering* — Chip Huyen. The best current map of the
production-LLM landscape (evals, RAG, fine-tuning vs. prompting, inference
optimization, architecture).

**Course:** Hamel Husain & Shreya Shankar's *AI Evals for Engineers* — the
strongest practical treatment of error analysis and eval design. Supplement
with Anthropic's docs on eval design and
[Simon Willison's blog](https://simonwillison.net/) (ongoing, the best running
commentary on LLM engineering practice).

**Reading in the gaps:** the
[*Site Reliability Engineering* book](https://sre.google/sre-book/table-of-contents/)
(free online) — just the chapters on SLOs, monitoring, and release engineering;
skim the rest.

**Rust thread:** *Programming Rust* priority chapters + finish *Zero To
Production*. Return to The Book's ch. 17 (async) properly — the Rust
curriculum told you to skim it the first time; now you need it. Finance-ladder
Project **6** (async market-data fetcher) is the ideal one-week tokio drill
before wiring concurrency into the runner.

**Project — eval runner v2:** concurrency (run evals in parallel — you now
know Rust's async story), a job log and reconciliation à la your
`parallelize-pipeline` skill, JUnit/JSON output so it gates CI, and regression
detection: store baseline scores, fail the build when a prompt/model change
drops them. Wire it into the repo's GitHub Actions. Stretch: statistical
treatment of flaky evals — when is a pass-rate drop significant? (You are
unusually qualified to answer.)

**Project (small):** an inference-economics memo for one real workload —
measure latency/cost/quality across model tiers, caching on/off, with your own
eval suite as the quality metric. One page, decision-first, like your
`stakeholder-narrative` skill prescribes.

**Done when:** a model or prompt change in your repo triggers CI that
quantifies regression, and you can defend the stats behind the pass/fail
threshold.

---

## Phase 3 · Months 7–9 — Model risk, AI governance & security (the finance moat)

Rare combination territory: engineers who can speak model validation,
validators who can ship. In a large financial institution this is the
differentiator.

**Primary sources (read them, not summaries):**

- **SR 11-7** — the Fed's model risk management guidance (~20 pages). The
  constitution of model risk in US banking.
- **NIST AI Risk Management Framework** (AI RMF 1.0 + the Generative AI
  profile).
- **EU AI Act** — a good secondary summary is fine here; know the risk tiers
  and what "high-risk" obligations imply for systems you'd build.
- **[OWASP Top 10 for LLM Applications](https://genai.owasp.org/)** — then
  Simon Willison's prompt-injection series for the attacker's-eye view.

**Rust thread:** *Rust for Rustaceans* ch. 1–4 and *Effective Rust* (Rust
curriculum Phase 4) — you're now designing APIs (the runner's plugin surface)
rather than fighting the borrow checker, which is exactly what these books are
for. Elective: Project **7** (PyO3) — exposing the eval runner or the Monte
Carlo pricer to Python is the highest-leverage work pattern you'll learn all
year.

**Project — red-team your own agents:** add adversarial evals to the repo —
prompt injection via tool results, data-exfiltration attempts through the
`warehouse-server` connection, jailbreaks of the agents' guardrails. Your eval
runner now does security regression testing; almost nobody has this.

**Project — model validation pack:** write a model card + SR 11-7-style
validation document for one real model or LLM system at work (or your
trading-strategies project as a stand-in): purpose, assumptions, limitations,
ongoing-monitoring plan, effective challenge. Then encode the pattern as
`assess-model-risk` and `red-team-agent` skills in the repo.

**Done when:** you could sit across from your firm's model risk management
team and negotiate what "validated" means for an LLM system — in their
language.

---

## Phase 4 · Months 10–12 — Staff-level leadership craft

The "lead" in the title. A distinct genre of writing and influence from
anything academic.

**Spine book:** *The Staff Engineer's Path* — Tanya Reilly. Then Will Larson's
*Staff Engineer* for the archetypes and promotion-adjacent politics. Optional:
Larson's *An Elegant Puzzle* if you start managing people.

**Practice, not courses (this phase is learned by doing):**

- Write one real **design doc/RFC** at work per month: context, options
  considered, decision, risks. Circulate it, absorb the feedback publicly.
- Run one **build-vs-buy analysis** for an AI capability (eval platform,
  guardrails vendor, RAG infra) — total cost, lock-in, exit path — and present
  it decision-first.
- Write **ADRs** for the significant choices in your own repos, retroactively.
  Cheap practice, real artifact.
- If your firm does incident reviews, volunteer to write one up.

**Rust thread:** Rust curriculum Phase 5 — the domain crates (Polars, ndarray,
Candle). If you want a final substantial build, this is where finance-ladder
Project **5** (Monte Carlo pricer) or the Project **8** capstone (backtesting
engine) fits; either doubles as raw material for a design doc and an ADR set.

**Project — close the loop:** encode the genre into the toolkit:
`write-design-doc` and `build-vs-buy` skills, so the discipline you practiced
becomes infrastructure you (and your team) reuse.

**Done when:** a design doc you wrote changed a real decision at work, and you
can name the three archetype directions (Reilly/Larson) and which one you're
growing toward.

---

## Continuous threads (all year)

- **Rust:** the reading order lives in [`CURRICULUM.md`](./CURRICULUM.md); this
  file only schedules it. After *Zero To Production*, go deeper with *Rust for
  Rustaceans* (Gjengset) and his live-coding videos. Every phase's project is
  your Rust gym.
- **Writing:** one public or internal post per month. Teaching is the fastest
  consolidation, and visibility is half the lead role.
- **Reading current practice:** Simon Willison (LLM engineering), Chip Huyen,
  Hamel Husain, Eugene Yan (applied ML systems), Will Larson (leadership). A
  feed of these five covers 90% of the field's signal.

## Deliberately excluded

More ML theory (you have it) · more languages beyond Rust/Go/Python ·
Kubernetes internals (concepts yes, YAML no) · leetcode-style algorithms —
your gap is systems design, not algorithms.

## The one-line version

**DDIA + distributed systems labs → eval engineering on your own toolkit →
SR 11-7 + red-teaming → design docs — with Rust as the vehicle throughout and
the eval runner as the artifact that ties all four phases together.**

Track progress in [`PROGRESS.md`](./PROGRESS.md).
