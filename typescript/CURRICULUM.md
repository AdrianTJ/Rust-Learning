# A TypeScript Curriculum — for building agent harnesses

The *why* behind each module. For the ordered list of things to actually do,
see [`CHECKLIST.md`](./CHECKLIST.md). For why this side quest exists at all,
see [`RATIONALE.md`](./RATIONALE.md).

Scoped for someone with twelve years of Python, no TypeScript, and exactly one
goal: build and read LLM agent harnesses. Everything outside that goal is cut.

---

## What's actually new for you

Almost nothing, which is the point. You know closures, higher-order functions,
generics-as-a-concept, and async. The genuinely new material is small:

1. **Types erase at runtime.** Pydantic validates when the program runs;
   TypeScript types are gone by then. This single fact is why Zod exists and
   why every harness has a validation layer at the model boundary. Internalize
   it in module 2 and the rest of the ecosystem stops looking redundant.
2. **Structural typing.** A value fits a type if its *shape* matches — no
   declaration, no inheritance, no registration. Closer to Python protocols
   than to `isinstance`, and more permissive than either.
3. **Discriminated unions + exhaustiveness.** The one feature with no good
   Python analog and the one you'll use most. Agent state, tool results, and
   stream events are all sum types, and the compiler will tell you when you
   forgot a case.

Everything else — `async`/`await`, generics, modules, iteration — is a syntax
remap of something you already do. Read it fast.

---

## Module 1 · Runtime setup — 1 hour

TypeScript historically meant a compile step. It doesn't anymore: `tsx`
transpiles on the fly, so `npx tsx src/index.ts` is your `python script.py`.

Setup lives in [`README.md`](./README.md). The only conceptual content is the
ESM/`.js`-extension gotcha, and the only reason it gets an hour is that
everyone loses forty minutes to it once.

**Done when:** a script runs, and you know why relative imports end in `.js`.

## Module 2 · TypeScript for Python developers — 3 hours

Read three chapters of the Handbook — *Everyday Types*, *Narrowing*,
*Generics* — and stop. The rest is for library authors.

What matters, in priority order:

- **Discriminated unions.** Model agent state as a union tagged by a literal
  field, `switch` on the tag, and let the compiler prove you handled every
  case (the `never` exhaustiveness trick is worth learning on day one).
- **`unknown` vs. `any`.** `any` disables the compiler silently; `unknown`
  forces you to narrow before use. Model output is `unknown`. Treating it as
  `any` is how harnesses get subtly broken.
- **Generics with one type parameter**, enough to write
  `defineTool<T extends ZodTypeAny>`. You do not need conditional or mapped
  types.
- **Promises vs. asyncio.** One event loop, no loop object, no
  `get_event_loop`, no sync/async library split. `Promise.all` is
  `asyncio.gather`; `Promise.race` is how you build timeouts;
  `Promise.allSettled` is what you actually want for tool dispatch, because one
  failing tool shouldn't sink the batch.

**Done when:** you can model agent state as a discriminated union and get a
compile error by deleting one `case`.

## Module 3 · Zod — 2 hours

The bridge across the runtime/compile-time gap. One declaration gives you three
things: a validator, a TypeScript type (`z.infer`), and a JSON Schema for the
model (`z.toJSONSchema()` in Zod 4 — the older `zod-to-json-schema` package is
no longer needed).

That triple is the whole reason TypeScript is pleasant for harness work. Your
tool's type, its runtime validation, and the schema the LLM sees can never
drift apart, because they're generated from one source.

Prefer `safeParse` over `parse` at the model boundary. A model sending bad
arguments is *expected traffic*, not an exception — it should become a tool
result the model can read and correct, not a thrown error.

Read the Zod docs end to end; they're short.

**Done when:** adding a field to a tool schema updates its type, its validation,
and what the model sees, with no other edit.

## Module 4 · The loop — 3 hours

The harness proper. Four responsibilities:

1. **History** — an append-only message array, including tool results.
2. **Dispatch** — model says "call this tool," you find it, validate args,
   run it.
3. **Termination** — final answer, max steps, or cancellation.
4. **Invariants** — the part tutorials skip. Every tool call the model makes
   *must* get a corresponding tool result appended, on every path: tool not
   found, validation failed, tool threw, timed out. Miss one and the next API
   call rejects the conversation as malformed. Structure the loop so it's
   impossible to `continue` without pushing a result.

Use the **Anthropic SDK**, not OpenAI's. Content blocks and `tool_use` /
`tool_result` differ meaningfully from `tool_calls`, and Anthropic's is the
format you'll meet at work.

**Done when:** multi-turn tool use works, and the unhappy paths leave the
history valid.

## Module 5 · Robustness — 4 hours

The largest module, because this is where a toy becomes a harness.

- **`AbortController`**, threaded through the model request *and* every tool
  call. Cancellation cannot be retrofitted — it has to be a parameter
  everywhere from the start. This is the single most important thing in the
  curriculum.
- **Timeouts**, per model call and per tool, built from `AbortSignal.timeout`
  or `Promise.race`.
- **Retries with exponential backoff and jitter** on 429 and 5xx, respecting
  `retry-after`. Every real harness has this and it's the difference between
  a demo and something you leave running.
- **Errors as data.** A tool that throws becomes a `tool_result` with
  `is_error: true`. The model reads it and adapts — often better than your
  error handling would have.

**Done when:** Ctrl-C mid-tool cancels cleanly and leaves a valid history, and
a deliberately-throwing tool produces a tool result rather than a crash.

## Module 6 · Streaming — 3 hours

Non-streaming is the toy path. Real harnesses stream, and streaming changes the
architecture: you don't receive a tool call, you receive *deltas* that assemble
into one — tool arguments arrive as JSON fragments that aren't parseable until
complete.

Learn: SSE event handling with the SDK's stream helpers, accumulating
`input_json_delta` fragments, and rendering text as it arrives. Then note where
partial-JSON parsing would let you show a tool call before it's finished — and
don't build it yet.

**Done when:** tokens render as they arrive and tool calls assemble correctly
from deltas.

## Module 7 · Safety — 3 hours

You are three months from reading SR 11-7 and the OWASP LLM Top 10. Build the
habits now.

- **Approval gating.** Classify tools by risk; require confirmation before the
  dangerous ones execute. This is the biggest architectural feature in Claude
  Code and it's absent from every tutorial. Design it as a hook the loop calls,
  not an `if` buried in a tool.
- **Subprocess done properly.** `spawn`, not `exec`: streamed output, a hard
  timeout, a kill signal that actually kills the process group, and output
  truncation. Models will run `find /` and hand you a gigabyte.
- **Path allowlists** for file tools, resolved and checked *after*
  canonicalization — `../` is the oldest trick there is.
- **Never `eval`.** Not `eval()`, not `new Function()`, not `vm` without a
  sandbox. Arbitrary code execution driven by model output is the whole
  attack surface in one line.

**Done when:** you have a bash tool that survives `find /`, asks before `rm`,
and cannot be talked out of its allowlist.

## Module 8 · Testing — 3 hours

Your spine project is an eval runner. A harness you can't test deterministically
is a harness you can't build an eval runner *on*.

Define the model client as an interface, then write a fake that replays scripted
responses. Now the whole loop — tool dispatch, validation failures, retries,
cancellation, max-steps — tests offline with no API key and no flakes.

Record a few real transcripts to replay as fixtures. `node --test` is built in;
you don't need a framework.

**Done when:** `npm test` exercises the full loop with no network access.

## Module 9 · Capstone — eval runner v1 — 8 hours

Milestones, in order:

1. **Discover** `agents/*/eval/*.eval.yaml`, parse them, print what was found.
   Validate the spec shape with Zod — same discipline as tool args.
2. **Drive** one agent through one harness for one spec.
3. **Score** the `expect` assertions off the event stream.
4. **Report** — a readable summary plus machine-readable JSON.
5. **Concurrency** — run specs in parallel with a bounded worker pool.
   Unbounded `Promise.all` over fifty specs will rate-limit you instantly;
   a semaphore is about fifteen lines.

Sequential and single-harness is fine through milestone 4. Correctness before
complexity — baseline first.

**Done when:** it grades all nine existing specs end to end.

## Module 10 · Read the real thing

Worth as much as the nine modules above it, and the actual payoff of the side
quest: **the best harnesses in existence are open source and written in the
language you just learned.**

Read Claude Code and Gemini CLI with three specific questions, because these
are what every tutorial omits and every real harness is organized around:

1. How does cancellation propagate from Ctrl-C to a running subprocess?
2. What happens when the context window fills — what gets summarized, what gets
   dropped, and who decides?
3. How does the permission prompt interrupt the loop without corrupting history?

Then skim the **MCP TypeScript SDK**. The spec's schema is authored in
TypeScript and generated to JSON Schema — which is, on its own, a decent
argument for this whole detour.

---

## Reading list

1. **TypeScript Handbook** — *Everyday Types*, *Narrowing*, *Generics* only.
   <https://www.typescriptlang.org/docs/handbook/>
2. **Total TypeScript** (Matt Pocock) — free tiers. The best TS material that
   exists. Use as reference, not a course. <https://www.totaltypescript.com/>
3. **Zod docs** — <https://zod.dev/>. Short; read all of it.
4. **Anthropic API docs** — tool use, streaming, prompt caching.
5. **MCP spec + TypeScript SDK** — <https://modelcontextprotocol.io/>. After
   module 4.
6. **Claude Code / Gemini CLI source** — module 10. The real curriculum.

## Timeline

Ten steps over 3–4 weeks at 8–10 hrs/week. Modules 1–4 are one week and feel
like review. Module 5 is the hard one and deserves its own week. Then the
capstone.

## The one-line version

**Learn the 20% of TypeScript that harnesses use, build one that cancels
cleanly and can't be talked into running `rm -rf`, then go read Claude Code.**
