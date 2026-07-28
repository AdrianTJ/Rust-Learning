# Why this side quest exists

Two questions: is the reasoning behind it sound, and was the curriculum that
prompted it any good. Short answers: **mostly**, and **the skeleton yes, the
code no.**

---

## Part 1 — The premise

The argument was: agentic harnesses are written in TypeScript, TypeScript is a
simple language, therefore spend a couple of months on TypeScript before Rust.

### "Harnesses are written in TypeScript" — mostly true

| Harness | Language |
|---|---|
| Claude Code | TypeScript / Node |
| Gemini CLI | TypeScript |
| Cline, Roo, Continue | TypeScript (VS Code extensions) |
| Amp (Sourcegraph) | TypeScript |
| Vercel AI SDK, LangChain.js, Mastra | TypeScript |
| MCP reference SDK | TypeScript (the spec schema is authored *in* TS) |
| **Codex CLI (OpenAI)** | **started TypeScript → rewritten in Rust, 2025–26** |
| **Goose (Block)** | **started Python → rewritten in Rust** |
| Aider, OpenHands | Python |

Two corrections. **Nous Research's Hermes is a model family, not a harness** —
trained and served in Python, so it isn't evidence either way. And **Codex is
the counterexample that matters**: OpenAI shipped the TypeScript version, hit a
ceiling, and rewrote the whole thing in Rust. Their reasons were zero-dependency
install (no Node v22+ requirement), native sandboxing bindings, no GC pauses,
lower memory. Goose made the same move from Python, for distribution as a single
binary.

So the accurate version of the premise is narrower and more useful:

> **TypeScript is where harnesses get prototyped and iterated. Rust is where
> they end up when they need to ship as a binary and hold a security boundary.**

Both serious rewrites in this space went *toward* Rust. That doesn't undercut
the plan — it means the sequence proposed here is the industry's own sequence,
and that the Rust track was never the wrong bet.

### "TypeScript is a simple language" — false in general, true where it counts

TypeScript's type system is among the most complex in mainstream use:
structural typing, conditional and mapped types, variance, inference that is
Turing-complete. People spend years on it.

But **the subset a harness needs is small** — unions, discriminated unions,
one-parameter generics, `async`/`await`, Zod. The complexity lives in library
authoring and React, and this curriculum does neither. A weekend gets you the
subset; the other three weeks are for the harness, not the language.

### "It lends itself to async" — true, and the strongest part of the argument

One event loop, no loop object, no `get_event_loop` ceremony, no sync-vs-async
library split, no GIL debate, and no colored-function trap at the ecosystem
level, because essentially everything I/O already returns a Promise.
`Promise.all` / `race` / `allSettled` plus `AbortController` cover almost all
harness concurrency. Python's asyncio can do all of it but makes you think
about it; TypeScript mostly doesn't.

### The corollary — the actual reason to do this

An LLM harness is close to the **worst possible first Rust project**. It's
dominated by async I/O, heterogeneous dynamic dispatch over tools, JSON of
unknown shape, streaming, and cancellation — precisely the list of things Rust
is hardest at (`async` traits, `Box<dyn Future + Send>`, `Pin`, untagged serde
enums, `Arc<Mutex<_>>` across tokio tasks).

Meanwhile the finance ladder — Monte Carlo pricing, CSV stats, the backtester,
Polars/ndarray — is CPU-bound, statically shaped, and data-parallel. That's Rust
at its most pleasant *and* most advantageous, and it plays to a math background.

So the split isn't a compromise, it's correct:

> **TypeScript for orchestration and I/O. Rust for numerics and systems.**
> The industry splits the same way.

---

## Part 2 — What changes, and what doesn't

**Three to four weeks, not two months.** The source curriculum estimates *twelve
hours* of material. Two months of calendar against twelve hours of content gets
filled with tutorials nobody needs. Thirty hours: ten on the language, twenty
building. Hence the exit criterion in [`README.md`](./README.md).

**Nothing in the Rust track has been edited.** The root `CHECKLIST.md`,
`CURRICULUM.md`, and `LEAD_AI_CURRICULUM.md` are untouched and this folder is
not a prerequisite for any of them. But one decision is worth making
deliberately, because it changes what the Rust year looks like:

> **Build eval runner v1 in TypeScript. Let the Rust version be v2 — a port
> against a known spec.**

The argument for it:

- v1 in TypeScript means discovering **what the artifact needs to be** while
  the language is out of the way. The current plan has you designing the runner
  and fighting the borrow checker simultaneously — that's its weakest joint.
- v2 in Rust becomes a rewrite with a specification, which is a better Rust
  exercise than greenfield and is literally the Codex and Goose trajectory.
- You end the year with the same tool in two languages plus a benchmark
  writeup — a better portfolio piece and a better design-doc subject (Phase 4)
  than either alone.
- Rust practice doesn't disappear, it relocates to the finance ladder, where
  it's the better tool anyway.

Net cost: about three weeks. Net gain: the spine project ships months earlier
and the rewrite has a spec.

If you take that decision, the root `CHECKLIST.md` step 17 and
`LEAD_AI_CURRICULUM.md` Phase 1 need re-pointing. Left alone for now on purpose
— that's an edit to the Rust track, and this is a side quest.

---

## Part 3 — Review of the source curriculum

*(`TypeScript_Agentic_Harness_Curriculum.md`, the document that prompted this.)*

**A good skeleton with a broken capstone.** Module selection for the first 20%
is right — types, discriminated unions, Promises, Zod, the loop. Nothing in the
outline is wrong to include. But the reference implementation doesn't compile,
has two protocol bugs, and the modules stop exactly where harness engineering
starts.

### Defects

1. **Syntax error in `main()`.** The final `console.log("` opens a
   double-quoted string with a raw newline inside it —
   `SyntaxError: Invalid or unexpected token`, verified with `node --check`.
   The other multi-line strings use backticks and are fine.

2. **The model never sees a usable schema.** `getOpenAiToolSpecs()` passes
   `(tool.schema as z.ZodObject<any>).shape` straight in as
   `parameters.properties` — that's Zod's internal object of `ZodString`
   instances, not JSON Schema, and `required` is omitted entirely. Module 3
   introduces `zodToJsonSchema` and the capstone never uses it. (Zod 4 has
   `z.toJSONSchema()` built in; the extra package isn't needed anymore.)

3. **The missing-tool branch corrupts the conversation.** `if (!tool)` logs and
   `continue`s *without appending a tool result*. Every `tool_call_id` needs a
   matching tool message; the next request 400s. The Zod-failure branch gets
   this right — that's the shape to copy. The unguarded `await tool.execute(...)`
   has the same hole: a throwing tool leaves history invalid.

4. **The generic is defeated at every use site.** `const sqlTool: AgentTool = {…}`
   annotates with the default `z.ZodTypeAny`, so `execute: async ({ query }) => …`
   receives `any`. The file demonstrates zero type safety in the exact place
   it's meant to demonstrate type safety. Fix with an inferring helper:
   ```ts
   const defineTool = <T extends z.ZodTypeAny>(t: AgentTool<T>): AgentTool<T> => t;
   ```
   Then note that a `Record<string, AgentTool>` re-widens it — TypeScript has no
   existential types, and the idiomatic answer (validate at the boundary, keep
   `execute` typed internally) is worth teaching rather than hiding.

5. **`response.choices[0].message` is unchecked** — which points at the tsconfig
   gap: `noUncheckedIndexedAccess` is missing, and it's the highest-value
   strictness flag beyond `strict` for this kind of code.

6. **Sequential loop under a "parallel tool dispatch" headline.** The overview
   promises parallel dispatch and context management; the capstone delivers
   neither and defers parallelism to an exercise. Calling any of it
   "production-ready" is a stretch too.

7. **`Function("return (" + expr + ")")` in the calculator.** Arbitrary code
   execution, in full Node scope, driven by model output — shipped unremarked in
   a harness tutorial. For someone three months from OWASP LLM Top 10 and
   SR 11-7, this is the worst thing in the document.

8. **Dependency drift.** It installs `@anthropic-ai/sdk` and never uses it —
   and Anthropic's content-block format is the one worth learning first here.
   Also `.optional().default(100)` is a confused ordering (`.default()` alone is
   what's meant), and `module: NodeNext` requires `.js` extensions on relative
   imports plus `"type": "module"` — the classic first-hour wall, unmentioned.

### Gaps — what's missing *is* the subject

Modules 1–5 reach a working toy. Everything separating a harness from a toy is
absent, including two things the overview explicitly promises:

- **Cancellation and timeouts** — `AbortController` through the model call and
  every tool. Cannot be retrofitted.
- **Streaming**, and assembling tool arguments from JSON deltas.
- **Context management** — promised, delivered nowhere. No token counting, no
  truncation, no compaction.
- **Retries with backoff** on 429/5xx.
- **Approval gating before dangerous execution** — the biggest architectural
  feature in Claude Code, entirely absent.
- **Subprocess done properly** — the exercise says `exec`; a harness needs
  `spawn` with streaming, timeout, kill signal, and truncation.
- **Testing with a fake model client** — disqualifying to omit, for someone
  whose spine project is an eval runner.
- **Structured logging** of the loop.

[`CURRICULUM.md`](./CURRICULUM.md) keeps modules 1–4 with the fixes above,
replaces the capstone, and adds modules 5–10 for the list above.

---

## The one-line version

**The premise holds — three to four weeks, not two months; the harness gets
built in TypeScript because that's what it's good at; Rust keeps the numerics
and gets the rewrite.**

---

*Sources: [InfoQ — Codex CLI goes native](https://www.infoq.com/news/2025/06/codex-cli-rust-native-rewrite/),
[devclass — OpenAI rewrites its AI coding tool in Rust](https://www.devclass.com/ai-ml/2025/06/02/nodejs-frustrating-and-inefficient-openai-rewrites-ai-coding-tool-in-rust/1619589),
[block/goose](https://github.com/block/goose),
[Model Context Protocol](https://modelcontextprotocol.io/), [Zod](https://zod.dev/).*
