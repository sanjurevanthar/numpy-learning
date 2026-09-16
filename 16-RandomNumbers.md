# Generating Random Numbers

The use of random number generation is an important part of the configuration and evaluation of many numerical and machine learning algorithms. Whether you need to randomly initialize weights in an artificial neural network, split data into random sets, or randomly shuffle your dataset, being able to generate random numbers (actually, repeatable pseudo-random numbers) is essential.

## Setting Up a Generator

```python
import numpy as np

rng = np.random.default_rng()  # the simplest way to generate random numbers
```

`default_rng()` is the recommended, modern entry point for random number generation in NumPy (it replaces the legacy `np.random.rand`/`np.random.randint` functions).

## Generating Random Integers

With `Generator.integers`, you can generate random integers from `low` (inclusive — note this is inclusive with NumPy) to `high` (exclusive). You can set `endpoint=True` to make the high number inclusive.

You can generate a 2 x 4 array of random integers between 0 and 4 with:

```python
rng.integers(5, size=(2, 4))
# array([[2, 1, 1, 0],
#        [0, 0, 0, 4]])  # may vary
```

## Generating Random Floats

`Generator.random` generates floats uniformly distributed between 0 and 1:

```python
rng.random(3)
# array([0.63696169, 0.26978671, 0.04097352])  # may vary

rng.random((3, 2))
# array([[0.01652764, 0.81327024],
#        [0.91275558, 0.60663578],
#        [0.72949656, 0.54362499]])  # may vary
```

## Reproducibility with a Seed

Passing a seed makes the "random" sequence repeatable — useful for debugging or sharing reproducible results:

```python
rng = np.random.default_rng(seed=42)
rng.integers(5, size=(2, 4))
# Produces the same output every time this code runs
```

Read more about random number generation in the [NumPy documentation](https://numpy.org/doc/stable/reference/random/index.html).

## See Also

- [Creating Matrices](15-CreatingMatrices.md) — using `ones()`, `zeros()`, and `random()` to initialize arrays.

---

## 📝 Study Notes

- **Prefer `default_rng()` over the legacy `np.random.seed()` / `np.random.rand()` API.** The old global-state API (still common in older tutorials) is being phased out in favor of the `Generator` object shown here, which avoids subtle bugs caused by shared global random state across your whole program.
- **`rng.integers(low, high)` is *inclusive* of `low` and *exclusive* of `high`** by default — the same convention as Python's `range()`. Set `endpoint=True` if you want `high` included too.
- **A seeded generator is reproducible *within the same NumPy version*** but isn't guaranteed to produce identical output across major NumPy version changes — don't rely on exact reproducibility for long-term archival, only for a single session or experiment.

**Try it yourself:**
1. Create two generators with the same seed (`np.random.default_rng(0)`), and confirm they produce identical sequences.
2. Generate a random 3x3 matrix of integers between 1 and 6 (inclusive) — like rolling a die 9 times.
