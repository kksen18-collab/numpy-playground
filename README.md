# numpy-playground

A repo where I poke numpy with a stick until it explains itself.

No library here. No tests, no CI badge, no `src/` layout pretending this is going to PyPI. Just four notebooks and the slow, dawning realisation that **everything in numpy is really an argument about shapes.**

---

## The notebooks

Run them in this order unless you enjoy confusion:

| # | Notebook | The plot |
|---|---|---|
| 1 | `numpy_basics.ipynb` | What `ndim` and `shape` actually mean. Then 10 cells of slicing, because slicing is 80% of numpy and 100% of the bugs. |
| 2 | `numpy_broadcasting.ipynb` | Two arrays of different shapes meet. Sometimes it's magic. Sometimes it's a `ValueError`. Learning which is which *is* the lesson. |
| 3 | `numpy_operations.ipynb` | Aggregates, the eternal `axis=0` vs `axis=1` coin-flip, and filtering ages into teenagers / adults / one suspiciously lonely 65-year-old. |
| 4 | `numpy_random.ipynb` | `default_rng` vs the legacy `np.random` gremlins, seeds, shuffling, and picking fruit out of a hat. |

Every code cell is commented. Not "this adds two numbers" commented — commented with the thing I actually had to look up.

---

## Some cells fail on purpose

In `numpy_broadcasting.ipynb`, cells 2 and 4 throw `ValueError` and that is **the intended output**. They are labelled `# FAILS:` with the reason.

Please do not fix them. They died for your education.

```
(4, 4) * (4, 2)  ->  ValueError
                     4 columns vs 2 columns, and 2 is not 1
```

Broadcasting's whole rulebook is basically: *line the shapes up from the right; each pair must either match or have a 1 in it.* That's it. That's the rule. Cells 2 and 4 are what happens when you forget it.

---

## Running it

[uv](https://docs.astral.sh/uv/) does everything:

```bash
uv sync                  # builds .venv, installs numpy
uv run jupyter lab       # or just open the .ipynb in VS Code
```

Python 3.13, numpy 2.x. The `.python-version` file pins the interpreter so uv doesn't have to guess.

> **If your notebook kernel looks haunted** — stale imports, numpy mysteriously missing — it's probably still pointing at an older venv that got replaced. Repoint the kernel at `.venv` and the ghosts leave.

`main.py` prints `Hello from numpy!` and does nothing else. It came free with `uv init` and I've grown fond of it.

---

## Things that cost me more time than they should have

- `array[:, 0]` gives you shape `(4,)`, **not** `(4, 1)`. It eats the axis. Every single time.
- A stop index is never included. `array[0:3]` is three rows, not four. Yes, still.
- Boolean masking *flattens*. `ages[ages < 18]` returns a flat 1-D array no matter how pretty your 2-D grid was. Use `np.where` if you want your shape back.
- Put one string in an integer array and numpy quietly converts **everything** to strings. The `0` sitting at the end of that 3×3×3 letter array? It's `'0'` now. It lives among the letters as one of them.
- `rng.shuffle(arr)` shuffles in place and returns `None`. `shuffled = rng.shuffle(arr)` gets you a crisp, confident `None`.

---

## Status

Perpetually unfinished, which is the correct state for a playground.
