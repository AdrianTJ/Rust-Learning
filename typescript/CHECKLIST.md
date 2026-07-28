# The TypeScript Checklist

**The tracker for the side quest.** Ten steps, in dependency order. The *why*
for each lives in [`CURRICULUM.md`](./CURRICULUM.md) — come back here for the
*next*.

Same line types as the Rust checklist, minus one:

- **Read** — docs or source
- **Build** — code in `typescript/harness/`
- **Write** — a journal entry

No **Drill** line. There's no Rustlings for TypeScript and you don't need one —
the compiler drills you while you build, and at twelve years in, exercises
would be busywork. If you're short on time, cut **Write** first and **Build**
never.

**Target:** 3–4 weeks at 8–10 hrs/week. Steps 1–4 are one week and will feel
like review. Step 5 is the hard one.

**Started:** _______

---

## Before you start

- [ ] Read [`RATIONALE.md`](./RATIONALE.md) — decide you actually agree with
      the plan before spending three weeks on it
- [ ] Agree to the exit criterion in [`README.md`](./README.md): capstone runs,
      study stops
- [ ] `node --version` ≥ 22
- [ ] Anthropic API key in `typescript/harness/.env` *(check it's gitignored)*

---

# Part I — The language, fast

*Goal: TypeScript stops being in the way. About one week.*

### 1 · Runtime setup

- [ ] **Build** scaffold `typescript/harness/` per the setup block in
      [`README.md`](./README.md)
- [ ] **Build** `src/index.ts` that prints something; `npx tsx src/index.ts` runs
- [ ] **Build** split into two files and import one from the other — hit the
      `.js`-extension wall on purpose, so it's never confusing again
- [ ] ✅ **Setup done** — script runs, no build step, editor shows type errors inline

### 2 · The language subset

- [ ] **Read** TS Handbook — *Everyday Types*, *Narrowing*, *Generics*. Stop there.
- [ ] **Build** model agent state as a discriminated union (`idle` / `thinking` /
      `executing_tool` / `error`); `switch` over it with a `never` exhaustiveness
      check
- [ ] **Build** delete one `case` and confirm the compiler catches it. This is
      the feature you came for.
- [ ] **Build** write something taking `unknown` and narrowing it — not `any`
- [ ] **Write** structural typing vs. Python protocols vs. `isinstance` — where
      the analogy breaks

### 3 · Async

- [ ] **Read** MDN on Promises and `async`/`await` *(skim — you know this shape)*
- [ ] **Build** `Promise.all`, `Promise.allSettled`, and `Promise.race` over
      three fake tools with different delays
- [ ] **Build** a `withTimeout(promise, ms)` helper from `Promise.race`
- [ ] **Write** asyncio vs. Promises — what ceremony disappeared, and what you
      lost along with it

### 4 · Zod

- [ ] **Read** the Zod docs, all of them
- [ ] **Build** a tool schema; derive its type with `z.infer` and its JSON
      Schema with `z.toJSONSchema()`
- [ ] **Build** add a field — confirm the type, the validation, and the model-facing
      schema all move together with no second edit
- [ ] **Build** use `safeParse`, not `parse`, and return the failure as text a
      model could act on
- [ ] **Write** why runtime validation is mandatory here and optional in Pydantic-land

---

# Part II — The harness

*Goal: a harness that survives contact with a real model. About two weeks.*

### 5 · The loop

- [ ] **Read** Anthropic docs — messages, content blocks, `tool_use` /
      `tool_result`
- [ ] **Build** a `defineTool` helper generic over the Zod schema, so `execute`
      gets real parameter types *(if your params are `any`, the generic is
      being defeated — check the annotation on the call site)*
- [ ] **Build** two tools: one pure (string manipulation), one that reads a file
- [ ] **Build** the loop — history, dispatch, validate, append result, terminate
      on final answer or max steps
- [ ] **Build** the four unhappy paths, each appending a tool result: tool not
      found, args failed validation, tool threw, max steps reached
- [ ] **Build** a test that runs a tool the model hallucinated and confirms the
      *next* API call still succeeds — this is the bug the source curriculum shipped
- [ ] ✅ **Loop done** — multi-turn tool use works and the unhappy paths leave
      history valid
- [ ] **Write** the invariant in one sentence, from memory

### 6 · Robustness

*The most important step in Part II. Don't rush it.*

- [ ] **Build** thread an `AbortSignal` through the model call and every tool —
      everywhere, as a parameter, before it's needed
- [ ] **Build** Ctrl-C cancels mid-tool, cleanly, leaving valid history
- [ ] **Build** per-call and per-tool timeouts
- [ ] **Build** retry with exponential backoff + jitter on 429/5xx, honouring
      `retry-after`
- [ ] **Build** a tool that always throws; confirm it becomes a tool result the
      model reads and works around, not a crash
- [ ] ✅ **Robustness done** — you can leave it running unattended
- [ ] **Write** why cancellation can't be retrofitted

### 7 · Streaming

- [ ] **Read** Anthropic streaming docs — event types
- [ ] **Build** stream text; render tokens as they arrive
- [ ] **Build** accumulate `input_json_delta` fragments into complete tool args
- [ ] **Build** confirm cancellation still works mid-stream *(this is where
      step 6 gets tested for real)*
- [ ] **Write** what streaming changed about the architecture, not just the UX

### 8 · Safety

- [ ] **Build** a risk classification on tools, and an approval hook the loop
      calls before executing a dangerous one
- [ ] **Build** a bash tool with `spawn`: streamed output, hard timeout, kill
      the process group, truncate output
- [ ] **Build** run `find /` through it — confirm truncation and timeout hold
- [ ] **Build** a file tool with a path allowlist checked after canonicalization;
      try to escape it with `../` and fail
- [ ] **Build** grep your own code for `eval` and `new Function`. Zero hits.
- [ ] ✅ **Safety done** — the harness asks before `rm` and can't be talked out
      of its allowlist
- [ ] **Write** the three attacks you'd try against your own harness

### 9 · Testing

- [ ] **Build** extract the model client behind an interface
- [ ] **Build** a fake client replaying scripted responses
- [ ] **Build** tests with `node --test` for: happy path, validation failure,
      tool throw, retry, cancellation, max steps
- [ ] **Build** record two real transcripts as fixtures
- [ ] ✅ **Tests done** — full loop covered, no API key, no network

---

# Part III — The capstone

*Goal: eval runner v1, in TypeScript.*

### 10 · Eval runner v1

- [ ] **Build** design sketch — discovery and scoring model. One page, no code.
- [ ] **Build** discover `agents/*/eval/*.eval.yaml`; validate the spec shape
      with Zod; print what was found
- [ ] **Build** drive one agent through one harness for one spec
- [ ] **Build** score the `expect` assertions off the event stream
- [ ] **Build** report — readable summary plus machine-readable JSON
- [ ] **Build** bounded-concurrency worker pool *(a semaphore, ~15 lines — not
      an unbounded `Promise.all`, which will rate-limit you instantly)*
- [ ] ✅ **Eval runner v1 done** — grades all nine specs end to end
- [ ] **Write** one post: what building v1 taught you about what v2 needs

### 11 · Read the real thing

- [ ] **Read** Claude Code source — how does cancellation reach a running
      subprocess?
- [ ] **Read** Claude Code source — what happens when context fills? What's
      summarized, what's dropped, who decides?
- [ ] **Read** Gemini CLI source — how does the permission prompt interrupt the
      loop without corrupting history?
- [ ] **Read** skim the MCP TypeScript SDK
- [ ] **Write** three things they do that your harness doesn't, and whether each
      is worth stealing

---

## Checkpoint — and the exit

- [ ] I can read an unfamiliar TypeScript harness without looking up syntax
- [ ] I know why every tool call needs a matching result, from memory
- [ ] Cancellation is a parameter in my code, not an afterthought
- [ ] I can test an agent loop with no network access
- [ ] Eval runner v1 grades all nine specs

**All five checked → stop.** Log the date, note what you'd port to Rust first,
and go open [`../CHECKLIST.md`](../CHECKLIST.md) at step 1.

**Finished:** _______
