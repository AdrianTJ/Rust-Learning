# The TypeScript Detour — verdict, and a revised plan

You proposed: TypeScript is the language of agentic harnesses, therefore spend
a couple of months on TypeScript before starting Rust.

**Verdict: the premise is right, the timeline is wrong, and the conclusion is
half right.** TypeScript belongs in the plan. Two months does not. And it
shouldn't be a *detour* before Rust — it should be a permanent second lane,
because TS and Rust are good at opposite halves of the work you want to do.

---

## Part 1 — Is the premise true?

### "Agentic harnesses are written in TypeScript" — mostly true

| Harness | Language |
|---|---|
| Claude Code | TypeScript / Node |
| Gemini CLI | TypeScript |
| Cline, Roo, Continue | TypeScript (VS Code extensions) |
| Amp (Sourcegraph) | TypeScript |
| Vercel AI SDK, LangChain.js, Mastra | TypeScript |
| MCP reference SDK | TypeScript (spec schema is authored *in* TS) |
| **Codex CLI (OpenAI)** | **started TypeScript → rewritten in Rust (2025–26)** |
| **Goose (Block)** | **started Python → rewritten in Rust** |
| Aider, OpenHands | Python |

Two corrections to your list. **Nous Research's Hermes is a model family, not a
harness** — it's trained and served in Python, so it isn't evidence for TS.
And **Codex is the counterexample that actually matters**: OpenAI shipped the
TS version, hit the ceiling, and rewrote the whole thing in Rust. Their stated
reasons were zero-dependency install (no Node v22+ requirement), native
sandboxing/security bindings, no GC pauses, lower memory.

So the honest version of your premise is narrower and more useful:

> **TypeScript is where agentic harnesses are *prototyped and iterated*. Rust is
> where they end up when they need to ship as a binary and hold a security
> boundary.**

Both of the serious rewrites in the space went *toward* Rust, not away from it.
That's not an argument against learning TS — it's an argument that your
existing Rust plan was never wrong, and that the industry sequence is exactly
the one you're proposing: TS first, Rust after.

### "TypeScript is a very simple language" — false as stated, true as it matters to you

TypeScript's type system is one of the most complex in mainstream use —
structural typing, conditional and mapped types, variance, inference that can
be Turing-complete. People spend years on it.

But **the subset a harness needs is genuinely small**: unions, discriminated
unions, generics with one type parameter, `async`/`await`, and Zod. You could
learn that subset in a weekend. The complexity lives in library authoring and
React, and you're doing neither.

### "TS lends itself naturally to async" — true, and it's the strongest part of your argument

This one you got exactly right, and it's underrated. Node has a single event
loop with no user-visible loop object, no `asyncio.run` / `get_event_loop`
ceremony, no sync-vs-async library split, no GIL debate, and no colored-function
trap at the ecosystem level (essentially everything I/O is already a Promise).
`Promise.all` / `Promise.race` / `AbortController` cover 90% of harness
concurrency. Python's asyncio can do all of it but makes you think about it;
TS mostly doesn't.

**The corollary you didn't state, which is the real reason to do this:** an LLM
harness is close to the *worst possible first Rust project*. It's dominated by
async I/O, heterogeneous dynamic dispatch over tools, JSON of unknown shape,
streaming, and cancellation — which is the precise list of things that are
hardest in Rust (`async` traits, `Box<dyn Future + Send>`, `Pin`, untagged
serde enums, `Arc<Mutex<_>>` across tokio tasks). Your current plan makes the
eval runner both your spine project *and* your Rust gym. That's the plan's
weakest joint, and this side quest is the fix.

---

## Part 2 — The revised sequencing

**Not "two months of TypeScript, then Rust." Instead:**

### 1. Time-box TypeScript to 3–4 weeks, ~30 hours

The curriculum you sent estimates **12 hours total**. That is the tell. Twelve
hours of material cannot fill two months; the gap would get filled with
tutorials you don't need. Budget 30 hours: ~10 on the language, ~20 building.
With twelve years of programming behind you, TS-the-language is a syntax
remap, not a learning curve.

**Hard exit criterion, written down now:** when the harness below runs your
existing eval specs end to end, TypeScript study stops and becomes maintenance.
The failure mode for this side quest is not "TS was a waste" — it's "two months
became eight and Rust never started."

### 2. Move eval runner v1 to TypeScript. Keep v2 in Rust.

This is the actual decision, and it makes your 12-month plan better rather than
just delaying it:

- v1 in TS means you discover **what the runner needs to be** while the language
  is out of the way. Right now you'd be designing the artifact and fighting the
  borrow checker simultaneously.
- v2 in Rust becomes a **port against a known spec**, which is a far better
  Rust exercise than a greenfield build — and is literally the Codex and Goose
  trajectory.
- You end the year with the same artifact in both languages and a real
  benchmark writeup. That's a better portfolio piece and a better design-doc
  subject (Phase 4) than either alone.

### 3. Rust practice relocates to where Rust is actually good

The finance ladder absorbs it, and improves as a result. Monte Carlo pricing
(Project 5), the CSV/stats work (Project 2), the backtester (Project 8),
Polars/ndarray — all CPU-bound, statically shaped, data-parallel. That is Rust
at its most pleasant and its most *advantageous*, and it plays directly to your
math background. Project 7 (PyO3) stays the highest-leverage item on the list.

So the tool split isn't a compromise, it's correct:

> **TypeScript for orchestration and I/O. Rust for numerics and systems.**
> The industry splits the same way, and so should the year.

### 4. Where it goes in the calendar

Insert as **Month 0** (weeks 1–4), before `CHECKLIST.md` step 1. DDIA can start
in parallel immediately — it's language-independent and it's the highest-value
book in the whole plan. Then:

| Months | Was | Now |
|---|---|---|
| 0 | — | **TS sprint + eval runner v1 (TS)**, DDIA ch. 1–4 in parallel |
| 1–3 | Book + Rustlings, runner v1 (Rust) | Book + Rustlings, **finance Projects 1–2** as the Rust gym |
| 4–6 | *Programming Rust*, runner v2 | *Programming Rust* + Z2P, **runner v2 = port v1 to Rust** |
| 7–9 | unchanged | unchanged — red-team evals land in the TS runner first, port after |
| 10–12 | unchanged | unchanged |

Net delay to the Rust track: about three weeks. Net gain: the spine project
ships four months earlier and the Rust rewrite has a specification.

---

## Part 3 — Review of the curriculum you were given

**Overall: a good skeleton with a broken capstone.** Module selection for the
first 20% is right — types, discriminated unions, Promises, Zod, the loop.
Nothing in the outline is wrong to include. But the reference implementation
does not compile, has two protocol bugs, and the modules stop precisely where
harness engineering begins.

### Bugs — the capstone as written does not run

1. **Syntax error, `main()`.** The final `console.log("` opens a double-quoted
   string with a raw newline inside it. That's `SyntaxError: Invalid or
   unexpected token` — verified. Use a backtick template literal or `\n`.
   (The other multi-line strings in the file *do* use backticks and are fine.)

2. **Tool specs are garbage — the LLM never sees a usable schema.**
   `getOpenAiToolSpecs()` passes `(tool.schema as z.ZodObject<any>).shape`
   directly as `parameters.properties`. That's Zod's internal object of
   `ZodString` instances, not JSON Schema. It also omits `required`. The
   curriculum introduces `zodToJsonSchema` in Module 3 and then never uses it —
   the fix is to use it (or Zod 4's built-in `z.toJSONSchema()`).

3. **Missing-tool branch corrupts the conversation.** In the `if (!tool)` path
   it logs and `continue`s *without pushing a tool result message*. The OpenAI
   API requires every `tool_call_id` in an assistant message to have a matching
   `tool` message. The next iteration 400s. (The Zod-failure branch gets this
   right — copy that shape.) The same hole exists around
   `await tool.execute(...)`: it's unguarded, so a throwing tool kills the loop
   and leaves the history in the same invalid state.

4. **The generic on `AgentTool` is defeated at every use site.**
   `const sqlTool: AgentTool = {...}` annotates with the default
   `z.ZodTypeAny`, so `execute: async ({ query }) => ...` receives `any`. The
   file demonstrates zero type safety in the exact place it's meant to
   demonstrate type safety. Fix with an identity helper that infers:
   ```ts
   const defineTool = <T extends z.ZodTypeAny>(t: AgentTool<T>): AgentTool<T> => t;
   ```
   Then note that the registry re-widens — storing differently-typed tools in
   one `Record` is a real limitation of TS (no existential types), and the
   idiomatic workaround (validate at the boundary, keep `execute` typed
   internally) is worth teaching rather than hiding.

5. **`response.choices[0].message` is unchecked.** Which points at a tsconfig
   gap: `noUncheckedIndexedAccess` is missing, and it's the single
   highest-value strictness flag beyond `strict` for this kind of code.

6. **Sequential loop under a "parallel tool dispatch" headline.** The overview
   promises parallel dispatch; the capstone `await`s inside a `for` and defers
   parallelism to an exercise. Fine as pedagogy, but the overview overclaims —
   as does calling any of this "production-ready."

7. **`Function("return (" + expr + ")")` in the calculator.** That is arbitrary
   code execution, in full Node scope, driven by model output. Shipping that
   unremarked in a harness tutorial — for someone whose Phase 3 is OWASP LLM
   Top 10 and SR 11-7 — is the worst thing in the document. Either use a real
   expression parser or make it an explicit, labelled lesson in what not to do.

8. **Dependency drift.** It installs `@anthropic-ai/sdk` and never uses it —
   and given where you work, Anthropic's content-block tool-use format is the
   one you should learn first; it differs meaningfully from OpenAI's
   `tool_calls`. Also: Zod 4 has `z.toJSONSchema()` built in, and
   `.optional().default(100)` is a confused ordering (`.default()` alone is
   what's meant). And `module: NodeNext` requires `.js` extensions on relative
   imports plus `"type": "module"` in `package.json` — the classic first-hour
   wall, unmentioned.

### Gaps — what's missing *is* the subject

The five modules take you to a working toy. Everything that distinguishes a
harness from a toy is absent, including two things the overview explicitly
promises:

- **Cancellation and timeouts** (`AbortController` threaded through the model
  call *and* every tool). This is the backbone of every real harness and it's
  the thing you cannot bolt on afterward.
- **Streaming**, and accumulating partial JSON for tool-call argument deltas.
  Non-streaming `chat.completions.create` is the toy path.
- **Context management** — promised in the overview, delivered nowhere. No
  token counting, no truncation, no compaction. This is the hardest unsolved
  part of harness design.
- **Retries with backoff** on 429/5xx, and idempotency across retries.
- **Permission / approval gating before dangerous tool execution** — the single
  biggest architectural feature in Claude Code, entirely absent.
- **Subprocess management done properly.** The "add a Bash tool" exercise says
  `child_process.exec`; a harness needs `spawn` with streamed output, a
  timeout, a kill signal, and output truncation, because models will happily
  run `find /`.
- **Testing the harness with a fake model client.** For someone whose entire
  spine project is an eval runner, being unable to test the loop deterministically
  is disqualifying.
- **Structured logging / tracing of the loop**, which is also what makes the
  eval runner's output legible.

---

## Part 4 — The revised curriculum

Keep Modules 1–4 as written (with the fixes above). Replace Module 5 and add
Modules 6–8. Total ≈ 30 hours over 3–4 weeks.

| Module | Focus | Hours | Done when |
|---|---|---|---|
| 1 | Runtime setup — `tsx`, `tsconfig`, ESM gotchas | 1 | `npx tsx src/index.ts` runs; you know why relative imports need `.js` |
| 2 | TS for Python devs — unions, discriminated unions, generics, Promises | 3 | You can model agent state as a discriminated union and get exhaustiveness checking from `switch` |
| 3 | Zod — schemas, `z.infer`, `z.toJSONSchema()`, `safeParse` | 2 | A tool's TS type and its JSON Schema both derive from one Zod declaration |
| 4 | The loop — history, dispatch, validation, termination | 3 | Multi-turn tool use against the Anthropic SDK, correct on the unhappy paths |
| **5** | **Robustness — `AbortController`, timeouts, retries with backoff, tool errors as messages** | **4** | Ctrl-C cancels mid-tool and leaves valid history; a thrown tool becomes a tool result, not a crash |
| **6** | **Streaming — SSE, tool-arg deltas, partial JSON** | **3** | Tokens render as they arrive; tool calls assemble from deltas |
| **7** | **Safety — approval gating, `spawn` with timeout/kill/truncation, path allowlists** | **3** | A bash tool that survives `find /` and asks before `rm` |
| **8** | **Testing — fake model client, recorded transcripts, deterministic loop tests** | **3** | The whole loop tests offline, no API key |
| **9** | **Capstone — eval runner v1 in TS** | **8** | Discovers `agents/*/eval/*.eval.yaml`, drives one agent, scores `expect` assertions, prints a report |

**Then, and this is worth as much as all nine modules: read Claude Code's and
Gemini CLI's source.** Both are TypeScript, both are open, and the payoff of
this whole side quest is arguably *reading* more than writing. Go looking
specifically for how they handle cancellation, context compaction, and the
permission prompt — the three things every tutorial omits and every real
harness is organized around.

### Reading list

- **TypeScript Handbook**, "Everyday Types" + "Narrowing" + "Generics" only —
  <https://www.typescriptlang.org/docs/handbook/>. Skip the rest.
- **Total TypeScript** (Matt Pocock), free beginner + type-transformations
  tiers — <https://www.totaltypescript.com/>. The best TS material that exists;
  use it as reference, not a course.
- **Zod docs** — <https://zod.dev/>. Short, read it all.
- **Anthropic Claude API docs**, tool use + streaming — the format you'll
  actually use.
- **MCP spec + TS SDK** — <https://modelcontextprotocol.io/>. Read after
  Module 4; the spec's schema is authored in TypeScript, which is its own
  argument for this whole detour.
- **Deliberately excluded:** React, Next.js, npm packaging, build tooling,
  bundlers, decorators, `class` inheritance patterns, everything frontend.

---

## The one-line version

**The premise holds — TS is where harnesses are built — but it's 3–4 weeks, not
two months; eval runner v1 moves to TypeScript and v2 becomes the Rust port;
and Rust relocates to the numerical projects where it's actually the better
tool. Net cost three weeks, net gain a shipped spine project and a Rust rewrite
with a spec.**
