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

- The steps: [`CHECKLIST.md`](../CHECKLIST.md), section **T**
- [`typescript-curriculum.md`](./typescript-curriculum.md): what each module covers and why
- [`typescript-rationale.md`](./typescript-rationale.md): the case for doing this at all, and a
  review of the harness curriculum that prompted it
- Setup: [`typescript/README.md`](../typescript/README.md)

## The exit criterion

Written down now, on purpose:

> **When the capstone harness completes one real task through an MCP server,
> TypeScript study stops.** Whatever's unfinished becomes reference material.

The failure mode for this side quest isn't "TypeScript was a waste of time."
It's "three weeks became eight months and the Rust track never restarted."
There is no shortage of TypeScript to learn; there is a shortage of reasons for
you to learn more of it than this.

## Deliberately excluded

React · Next.js · any frontend · bundlers and build tooling · npm publishing ·
decorators · class inheritance patterns · anything about the DOM. If a tutorial
mentions a browser, close it.
