# The ML Systems Track

**Train models yourself, then put them behind an API someone else could call.**

A separate thread from Rust and from TypeScript. Nothing here is a prerequisite
for either, and nothing there is a prerequisite for this. It's in Python on
purpose: you already know the language, so all the effort goes into the two
gaps this track exists to close.

## The two gaps

1. **Model training, done with your own hands.** You have the theory: you can
   derive backprop, you know what Adam does, and you can explain the
   bias–variance tradeoff to anyone. What's missing is the *craft*: getting a
   loss curve to go down, knowing what to do when it doesn't, and seeing all
   the moving parts (data loader, autograd, optimizer state, checkpoints, the
   GPU) as code rather than as boxes on a diagram.
2. **Systems design, especially APIs.** How a service is shaped: request and
   response contracts, versioning, timeouts, retries, idempotency, rate
   limits, batching, caching, observability. A CS degree spreads this across
   three or four courses and a first job. A math degree skips it.

The plan is one sequence that closes both: **build a framework → train with the
real one → serve what you trained.** Every part produces the input to the next.

## The files

- **[`CHECKLIST.md`](./CHECKLIST.md)** is the tracker: the steps in order.
  Start here.
- [`CURRICULUM.md`](./CURRICULUM.md) explains what each part covers and why
  it's on the list.

## The resources, and how they fit together

| Resource | What it is | Role here |
|---|---|---|
| [TinyTorch](https://mlsysbook.ai/tinytorch/) (Harvard CS249r) | 20 modules: build a PyTorch-shaped framework in NumPy, from tensors to transformers to optimization | **The build**, Parts I and III |
| [TrenTorch](https://github.com/TrenTorch/TrenTorch) | A re-implementation of TinyTorch's 20-module CLI, plus 350+ graded questions that run in the browser at trentorch.com | **The drill**, the Rustlings of this track |
| [PyTorch](https://pytorch.org/) | The real thing | **The tool**, Parts II–IV |
| [*Machine Learning Systems*](https://mlsysbook.ai/) | TinyTorch's companion textbook | Reference for the *why* |

**One thing to know before you start: TrenTorch's CLI and TinyTorch are the same
curriculum.** TrenTorch's README says so itself: it is "built on the curriculum
and foundation of TinyTorch". The module list, the four parts, and the
historical milestones match one for one. Doing both CLIs means building the same
framework twice.

So pick one CLI. This plan uses **TinyTorch's**, for three reasons: it's the
original, it has a textbook and a paper behind it, and it's the one the
textbook's chapters refer to. TrenTorch's **web question bank** is the part
TinyTorch doesn't have, so it becomes the drill line: short, auto-graded
reps on the topic you're building that week. If you'd rather go the other way
round (TrenTorch CLI, TinyTorch textbook), the checklist works unchanged because
the module numbers are the same.

## What TinyTorch won't teach you

TinyTorch runs on NumPy and a CPU, at toy scale. It shows you how a framework
works, which is half of gap 1. It won't make you good at training, because
the hard parts of training only show up at real scale and on real hardware:
data that doesn't fit in memory, a GPU sitting idle while the data loader
struggles, a loss that goes to NaN at step 4,000, choosing a learning rate.
That's why Part II exists, and why it uses PyTorch rather than more NumPy.

## Deliberately excluded

More ML theory (you have it) · reinforcement learning · writing your own CUDA
kernels (you'll *read* what they do in TinyTorch module 17, which is enough) ·
Kubernetes · MLOps platform shopping (Kubeflow, SageMaker, Vertex) · LeetCode.
Learn what a feature store *is*; don't stand one up.

## Setup

Python 3.10+.

```bash
cd ml-systems
curl -sSL mlsysbook.ai/tinytorch/install.sh | bash
cd tinytorch && source .venv/bin/activate && tito setup
```

That puts your TinyTorch work inside this repo, and `.venv/` is already
gitignored. If the installer creates a nested `.git` inside `tinytorch/`,
delete it so your solutions get committed here; otherwise git will treat the
folder as an opaque submodule. The module workflow (`tito` commands to start,
test, and complete a module) is documented at
<https://mlsysbook.ai/tinytorch/tito/modules.html>.

For the TrenTorch drills, sign in at trentorch.com. Nothing to install.

For Part II onward you need a GPU some of the time. Free notebook tiers
(Colab, Kaggle) are enough through step 9. After that, rent by the hour. A
single mid-range GPU for an afternoon is plenty for everything in this track.
