# The ML Systems Checklist

**The tracker for this track.** Steps in dependency order. The *why* lives in
[`CURRICULUM.md`](./CURRICULUM.md); come back here for the *next*.

Line types:

- **Read**: book, docs, or paper
- **Build**: TinyTorch modules, or your own code
- **Drill**: TrenTorch web questions on the current topic
- **Train**: a training run you start, watch, and explain
- **Write**: a journal entry, or the design doc

If you're short on time, **cut Drill first and Train never.** Training runs are
the gap this track exists to close.

Nothing is dated. Go at whatever pace you actually go at.

**Started:** _______

---

## Before you start

- [ ] Read [`README.md`](./README.md), especially *what TinyTorch won't teach you*
- [ ] Python ≥ 3.10; TinyTorch installed in `ml-systems/tinytorch/`; `tito setup` passes
- [ ] TrenTorch web account
- [ ] GPU access sorted for later: Colab or Kaggle is enough to start

---

# Part I: Build the framework

*Goal: every piece of a training loop is code you wrote. TinyTorch modules 01–08.*

### 1 · Tensors to layers

- [ ] **Build** TinyTorch 01 Tensor, 02 Activations, 03 Layers, 04 Losses
      *(go fast; this is linear algebra you could teach)*
- [ ] **Drill** TrenTorch linear-algebra questions
- [ ] **Build** ✅ **Milestone: 1958 Perceptron**

### 2 · Data

- [ ] **Build** TinyTorch 05 Dataloader
- [ ] **Write** why batching exists, beyond "the GPU likes it"

### 3 · Autograd

*The most important step in Part I. Don't rush it.*

- [ ] **Build** TinyTorch 06 Autograd
- [ ] **Read** the *ML Systems* textbook section on autodiff / training that
      module 06 points to
- [ ] **Drill** TrenTorch backprop / autograd questions
- [ ] **Write** draw the graph for a two-layer MLP + cross-entropy; mark which
      tensors are saved for backward, and why

### 4 · Optimizers and the loop

- [ ] **Build** TinyTorch 07 Optimizers: SGD, momentum, Adam
- [ ] **Build** TinyTorch 08 Training
- [ ] **Build** ✅ **Milestone: 1969 XOR** and ✅ **Milestone: 1986 MLP**
- [ ] **Write** what Adam keeps in memory per parameter, and what that costs at
      1B parameters

### 5 · Checkpoint: framework

- [ ] I can explain what `loss.backward()` does, step by step, without hand-waving
- [ ] I know what optimizer state is and where it lives

---

# Part II: Train for real

*Goal: you've trained a model to a target you set in advance, and can explain
every spike in the loss curve.*

### 6 · Translate to PyTorch

- [ ] **Read** PyTorch *Learn the Basics* tutorial
- [ ] **Build** rebuild your Part I MLP in PyTorch; for each piece, name the
      TinyTorch module it replaces
- [ ] **Train** same data, both frameworks; the curves should match
- [ ] **Write** three things PyTorch does that your framework didn't

### 7 · The procedure

- [ ] **Read** Karpathy, *A Recipe for Training Neural Networks*
- [ ] **Train** deliberately: overfit one batch; check initial loss against
      `log(num_classes)`; LR range test; then train properly
- [ ] **Train** break it on purpose (LR ×100, forgot `zero_grad`, shuffled
      labels) and record what each failure looks like on the curve
- [ ] **Write** your own one-page training checklist. You'll reuse it.

### 8 · Language models, built by hand first

- [ ] **Build** TinyTorch 09 Convolutions *(+ ✅ **Milestone: 1998 CNN**)*
- [ ] **Build** TinyTorch 10 Tokenization, 11 Embeddings
- [ ] **Build** TinyTorch 12 Attention, 13 Transformers
- [ ] **Drill** TrenTorch transformer questions
- [ ] **Build** ✅ **Milestone: 2017 Transformer**

### 9 · makemore

- [ ] **Read/Build** *Zero to Hero*, makemore lectures *(skip micrograd; you
      built it in step 3)*
- [ ] **Train** a character-level model; hit the validation loss from the lecture

### 10 · Reproduce GPT-2 (124M)

*The core of the track.*

- [ ] **Read/Build** *Zero to Hero*, "Let's build GPT" and "Let's reproduce
      GPT-2", against `build-nanogpt`
- [ ] **Train** pick the target validation loss *before* the run, then hit it
- [ ] **Train** mixed precision, gradient accumulation, LR warmup + cosine
      schedule, gradient clipping. Turn each on separately and measure what it buys
- [ ] **Train** kill the run halfway; resume from checkpoint; the curve must
      not jump
- [ ] **Train** experiment tracking for every run from here on
- [ ] ✅ **GPT-2 reproduced**: target hit; you can explain every spike
- [ ] **Write** one post: what training a GPT taught you that the theory didn't

### 11 · Checkpoint: training

- [ ] I can diagnose a stalled loss curve without guessing
- [ ] I know where the memory goes: params, grads, optimizer state, activations
- [ ] I've resumed a run from a checkpoint cleanly

---

# Part III: Serve it

*Goal: the model you trained is a service someone else could call, and you can
defend its design.*

### 12 · System design vocabulary

- [ ] **Read** Xu, *System Design Interview Vol. 1*: scale from zero to
      millions, back-of-the-envelope estimation, the design framework
- [ ] **Write** a capacity estimate for serving your GPT to 1,000 users

### 13 · The API

- [ ] **Build** FastAPI service around your Part II model: typed request and
      response schemas, explicit errors, versioned path (`/v1/...`)
- [ ] **Build** health check that actually loads and runs the model
- [ ] **Build** per-request timeouts that stop GPU work
- [ ] **Write** what a `/v2` would need to change, and how `/v1` clients survive it

### 14 · Making it a service

- [ ] **Read** Xu: rate limiter; consistent hashing
- [ ] **Build** token-bucket rate limiting, per client
- [ ] **Build** idempotency keys on any endpoint that does work worth not
      repeating
- [ ] **Build** structured logs with request IDs; p50/p95/p99 latency
- [ ] **Build** load test it; find the knee in the latency curve

### 15 · Batching and inference optimization

- [ ] **Build** dynamic batching. Measure the latency/throughput tradeoff
- [ ] **Build** TinyTorch 14 Profiling, 15 Quantization, 16 Compression
- [ ] **Build** TinyTorch 17 Acceleration, 18 Memoization, 19 Benchmarking
- [ ] **Build** ✅ **Milestone: MLPerf-style optimization**
- [ ] **Build** apply one of 15–18 to your served model; re-run the load test
- [ ] **Write** the before/after numbers, and why they moved

### 16 · ML systems design

- [ ] **Read** Huyen, *Designing Machine Learning Systems*: deployment &
      prediction service; distribution shifts & monitoring
- [ ] **Build** one monitoring signal for drift on your service's inputs

### 17 · Capstone

- [ ] **Build** TinyTorch 20 Capstone
- [ ] **Write** ✅ **design doc** for your service: requirements, API contract,
      capacity estimate, batching/caching decisions with load-test evidence,
      failure modes, what changes at 100× traffic
- [ ] ✅ **Service done**: callable from the docs alone; degrades gracefully
      under 10× design load

### 18 · Checkpoint: systems

- [ ] I can whiteboard a model-serving system and defend each box
- [ ] I can explain idempotency, backpressure, and p99 with my own examples
- [ ] I can read an API and say what will break when it changes

---

# Off the main line

Do these with genuine slack; skip without guilt.

- [ ] **Train** a model on your own domain data (market data, a work dataset)
      with an honest out-of-sample eval
- [ ] **Read** *The Ultra-Scale Playbook*; **Train** your GPT on multiple GPUs
      with DDP, then FSDP; measure scaling efficiency
- [ ] **Read** Stas Bekman, *ML Engineering Open Book*, as reference
- [ ] **Build** ✅ **Milestone: 2024 Custom Kernels** (TinyTorch)
- [ ] **Read** Google AIP (<https://google.aip.dev/>), as reference for API shape

---

# Stumbling blocks

_Running log: NaNs, shape errors, loss curves that made no sense, and what they
turned out to be._

-
