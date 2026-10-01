# An ML Systems Curriculum: training and serving

The *why* behind each part. For the ordered list of things to actually do, see
[`CHECKLIST.md`](../CHECKLIST.md) (section **M**). For the resources and setup, see
[`ml-systems-overview.md`](./ml-systems-overview.md).

Scoped for an applied mathematician who knows ML theory cold, writes Python
fluently, and has never trained a network end to end or designed a service.

---

## What's actually new for you

Not the math. Budget your energy for:

1. **Autograd as a data structure.** You know the chain rule. The new part is
   that a framework builds a graph *at runtime* while your forward pass
   executes, and `backward()` is a topological sort over it. Once you've
   built it (TinyTorch module 06), every PyTorch error message about
   `requires_grad`, in-place ops, or "trying to backward through the graph a
   second time" becomes readable.
2. **Training as debugging.** In theory you pick a model and optimize it. In
   practice it fails silently, so the loss goes down a bit and stalls, and
   nothing tells you why. The skill is a *procedure*: overfit one batch,
   check the initial loss against `log(num_classes)`, plot everything, change
   one thing at a time. Karpathy's "Recipe" (Part II) is that procedure written
   down.
3. **Memory and throughput as first-class constraints.** On paper a model is
   a function. On hardware it's parameters + gradients + optimizer state +
   activations, all fighting for the same memory, and it's fed by a data
   pipeline that's usually slower than the GPU. This is where "ML" becomes
   "ML *systems*", and it's the topic of TinyTorch Part IV.
4. **Contracts between services.** An API is a promise: this input shape gets
   this output shape, within this latency, and here's what happens when it
   doesn't. Idempotency, timeouts, retries, and versioning are what keep that
   promise when the network and your clients misbehave. Nothing in a math
   degree touches it, and it's the core of every system-design conversation.

---

## Part I · Build the framework: TinyTorch Foundations (modules 01–08)

Tensor → activations → layers → losses → dataloader → **autograd** →
optimizers → training loop, all in NumPy, all code you wrote.

This gives you the mental model for everything later. When PyTorch does
something surprising in Part II, you'll know which of your eight modules it
corresponds to, and you'll be able to guess what's going on underneath.

Go fast on 01–04 (it's linear algebra you could teach). Go **slow on 06
(autograd)** and **07 (optimizers)**: those are where the framework's design
decisions live, and where your theory finally becomes code. Module 08 is
your first training loop.

**Milestones:** TinyTorch's 1958 Perceptron, 1969 XOR, and 1986 MLP
milestones. They're small, and they're the first time *your* framework
learns something.

**Drill:** TrenTorch web questions for the topic of the module you're on.

**Done when:** you can draw the autograd graph for a two-layer MLP with
cross-entropy loss and say which tensors are retained for the backward pass and
why.

## Part II · Train for real: PyTorch (the core of the track)

This part closes gap 1. Three moves:

**1. Translate.** Rebuild your Part I MLP in PyTorch: `nn.Module`,
`torch.autograd`, `DataLoader`, `torch.optim`. Each one maps onto a module you
wrote. Read the official PyTorch *Learn the Basics* tutorial alongside it.

**2. The procedure.** Read Karpathy's
[*A Recipe for Training Neural Networks*](https://karpathy.github.io/2019/04/25/recipe/)
before training anything non-trivial, and then re-read it after your first
failed run. It's the most useful thing ever written for someone who knows the
theory and hasn't done the practice.

**3. Train something real.** Work through Karpathy's
[*Neural Networks: Zero to Hero*](https://karpathy.ai/zero-to-hero.html),
starting at *makemore*. You can skip *micrograd*, since TinyTorch module 06
already covered it. It ends at **reproducing GPT-2 (124M)**, with
[`build-nanogpt`](https://github.com/karpathy/build-nanogpt) as the reference
code. Before you start the GPT, do TinyTorch modules 09–13 (convolutions,
tokenization, embeddings, attention, transformers) so attention is something
you've built in NumPy rather than something you're watching.

What you're actually learning in this part: loss curves, learning-rate warmup
and schedules, gradient clipping, mixed precision, gradient accumulation,
checkpointing and resuming, evaluation during training, and experiment
tracking. That's the craft, and it's the reason Part II is the biggest one.

**Stretch:** train on something from your own domain. A small sequence model on
market data, with an honest out-of-sample evaluation, is better
portfolio material than another Shakespeare GPT, because it'll go wrong in more
interesting ways.

**Done when:** you've trained a model from random init to a target validation
loss you picked in advance, resumed it from a checkpoint without the loss
jumping, and can explain every spike in the curve.

## Part III · Serve it: systems design and APIs

This part closes gap 2. The model you trained in Part II becomes a service.

**The build.** Put the model behind an HTTP API (FastAPI is the obvious choice
in Python; Pydantic models give you the request/response contract). Then add,
one at a time, everything that separates a demo from a service:

- **The contract.** Typed request and response schemas, explicit error
  responses, a version in the path (`/v1/generate`). Changing the contract
  without breaking clients is a design problem, and it's the one you'll be
  asked about.
- **Timeouts and cancellation.** A request that's taking too long should stop
  using the GPU.
- **Idempotency.** A client that retries a request after a dropped connection
  shouldn't cause double work or double billing. Stripe's idempotency keys are
  the canonical example.
- **Rate limiting.** Token bucket, per client.
- **Batching.** Collect requests for a few milliseconds and run them together.
  This is the single biggest throughput lever for model serving, and it's a
  latency/throughput tradeoff you can measure.
- **Observability.** Structured logs, request IDs, p50/p95/p99 latency,
  and a health check that actually checks something.
- **Load test it.** Find the knee in the latency curve. Then use TinyTorch
  Part IV (modules 14–19: profiling, quantization, compression,
  acceleration, memoization/KV-caching, benchmarking) to move it, and
  explain the change in numbers.

**The reading.** Two books, read selectively:

- ***System Design Interview, Vol. 1*** (Alex Xu). Ignore the title; it's the
  most efficient primer on the *vocabulary* of system design that exists.
  Prioritize the chapters on scaling from zero to millions of users,
  back-of-the-envelope estimation, the design framework, rate limiting,
  and consistent hashing. Skim the case studies.
- ***Designing Machine Learning Systems*** (Chip Huyen, 2022). The ML-specific
  layer: training data, deployment and prediction services, distribution
  shift and monitoring. Read the deployment and monitoring chapters properly.
  (Not the same book as Huyen's *AI Engineering* in the Lead AI track; that one
  is about LLM applications.)
- **Reference, not reading:** Google's API Improvement Proposals at
  <https://google.aip.dev/>, for when you want to know how a large org
  answers "how should this endpoint be shaped?"

**Capstone:** TinyTorch module 20, plus a **one-page design doc** for your
service: requirements, the API contract, capacity estimate, the batching and
caching decisions with the load-test numbers that justify them, failure modes,
and what you'd change at 100× traffic. The writing is what turns the build
into something you can talk through in a design conversation.

**Done when:** someone could call your service from their own code using only
the API docs, it degrades gracefully under 10× the load it was designed for,
and the design doc defends every number in it.

## Part IV · Distributed systems: DDIA + MIT 6.5840

*Moved here from the Rust track (2026-10). It used to sit behind 13 steps of
Rust, which made the systems-design gap wait on the slowest track in the repo.*

Part III teaches you to design a service. This part teaches you how the
databases and services it sits on behave when machines fail, the network
partitions, and clocks disagree. That's the difference between drawing a
system-design diagram and defending one.

**Spine book:** *Designing Data-Intensive Applications*, **2nd edition**
(Kleppmann & Riccomini, 2026). About a chapter a week, with notes. The 2nd
edition opens with two new chapters (trade-offs in data systems architecture;
nonfunctional requirements), so its chapter numbers don't match the 1st
edition's. The checklist names chapters by topic for that reason. Read it all,
but note two links to this track: *encoding and evolution* is API versioning
for data, and *replication*/*consistency* are what your Part III service's
database is quietly doing for you.

**Course:** [MIT 6.5840](https://pdos.csail.mit.edu/6.824/) (formerly 6.824),
lectures and labs free online. Labs 1–4: MapReduce, a key/value server, Raft,
and a fault-tolerant key/value service on your own Raft. **The labs are in Go.**
If Go is new to you, *A Tour of Go* takes a weekend and is enough. The labs use
a small, plain subset, and goroutines plus channels are the only new ideas.

Lab 2 deserves a note: it's an API-semantics lab in disguise. Clients retry, and
a `Put` whose reply got lost returns `ErrMaybe`. That's the idempotency problem
from Part III, made rigorous.

**When to start:** the reading has no dependency on Parts I–III. If Part II's
training runs leave you waiting on a GPU, that's a good time to read DDIA. The
labs are best after Part III, once you've built a service and felt why these
guarantees matter.

**Done when:** you can explain linearizability vs. eventual consistency with a
real example, and your Raft passes its tests.

## Off the main line · Scale

Multi-GPU training is where "ML systems" becomes a specialty of its own. Only
if you want to go there:

- [*The Ultra-Scale Playbook*](https://huggingface.co/spaces/nanotron/ultrascale-playbook)
  (Hugging Face). Data, tensor, pipeline, and context parallelism, ZeRO/FSDP,
  explained with measurements.
- [*Machine Learning Engineering Open Book*](https://github.com/stas00/ml-engineering)
  (Stas Bekman). The field manual: hardware, networking, debugging training
  at scale. Reference only.
- **Build:** take your Part II training run to multiple GPUs with DDP, then
  FSDP. Measure scaling efficiency.

---

## How this connects to the other tracks

It doesn't, on purpose. Nothing here waits on Rust or TypeScript, and nothing
there waits on this.

## The one-line version

**Build autograd in NumPy, train a GPT in PyTorch until you can explain its loss
curve, serve it behind an API you can defend, then learn what the systems
underneath it guarantee.**
