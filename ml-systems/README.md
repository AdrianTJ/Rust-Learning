# ML systems track: workspace

TinyTorch, your PyTorch training code, and the serving code all go here. Steps
are in [`../CHECKLIST.md`](../CHECKLIST.md), section **M**.

## Setup

Python 3.10+.

```bash
# from ml-systems/
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
(Colab, Kaggle) are enough through M8. After that, rent by the hour. A
single mid-range GPU for an afternoon is plenty for everything in this track.

Suggested layout once you get going: `tinytorch/` (the installer creates it),
`training/` (Part II), `service/` (Part III), `6.5840/` (Part IV labs).
