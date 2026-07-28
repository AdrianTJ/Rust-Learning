# The TypeScript Side Quest

A self-contained detour: **3–4 weeks, ~30 hours**, ending in a working agentic
harness. Separate from the Rust track on purpose — nothing in this folder is a
prerequisite for anything in the repo root, and nothing in the root is a
prerequisite for this.

## Why this exists

The harnesses you want to build things like are written in TypeScript — Claude
Code, Gemini CLI, Cline, Amp, the AI SDK, the MCP reference SDK. The language
subset they use is small and you already know how to program. Three weeks buys
you the ability to *read* those codebases fluently and to prototype a harness
in an afternoon.

It is a side quest, not a career change. Rust remains the differentiator.

## The files

- **[`CHECKLIST.md`](./CHECKLIST.md)** — the tracker. Ten steps, in order.
  Start here.
- [`CURRICULUM.md`](./CURRICULUM.md) — what each module covers and why it's in
  the list
- [`RATIONALE.md`](./RATIONALE.md) — the case for doing this at all, and a
  review of the harness curriculum that prompted it

## The exit criterion

Written down now, on purpose:

> **When the capstone harness runs an eval spec end to end, TypeScript study
> stops.** Whatever's unfinished becomes reference material.

The failure mode for this side quest isn't "TypeScript was a waste of time."
It's "three weeks became eight months and the Rust track never restarted."
There is no shortage of TypeScript to learn; there is a shortage of reasons for
you to learn more of it than this.

## Deliberately excluded

React · Next.js · any frontend · bundlers and build tooling · npm publishing ·
decorators · class inheritance patterns · anything about the DOM. If a tutorial
mentions a browser, close it.

## Setup

```bash
mkdir -p typescript/harness && cd typescript/harness
npm init -y
npm pkg set type=module
npm install -D typescript @types/node tsx
npm install zod @anthropic-ai/sdk dotenv
```

`tsconfig.json` — the version from the source curriculum, plus the two flags it
was missing:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "types": ["node"],
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist"
  },
  "include": ["src/**/*"]
}
```

Run with `npx tsx src/index.ts`. No build step.

> **First-hour wall:** with `module: NodeNext` and `"type": "module"`, relative
> imports need a `.js` extension even though the file is `.ts` —
> `import { runLoop } from "./loop.js"`. It looks wrong. It is correct. This
> trips up everyone once.
