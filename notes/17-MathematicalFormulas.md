# Working with Mathematical Formulas

The ease of implementing mathematical formulas that work on arrays is one of the things that makes NumPy so widely used in the scientific Python community.

## Example: Mean Squared Error

The **mean squared error (MSE)** formula is a central formula used in supervised machine learning models that deal with regression:

**MSE = (1/n) × Σ(prediction − label)²**

Where `n` is the number of data points, and the sum is taken over every prediction/label pair.

Implementing this formula is simple and straightforward in NumPy:

```python
import numpy as np

def mse(predictions, labels):
    n = predictions.size
    return ((predictions - labels) ** 2).sum() / n
```

## Why This Works So Well

What makes this work so well is that `predictions` and `labels` can contain one or a thousand values — they only need to be the same size.

```python
predictions = np.array([1.5, 2.1, 3.7])
labels = np.array([1.0, 2.0, 4.0])

mse(predictions, labels)
```

## How It Works, Step by Step

In this example, both the `predictions` and `labels` vectors contain three values, meaning `n` has a value of three.

1. **Subtract** — NumPy subtracts each label from its corresponding prediction, element-wise: `predictions - labels`.
2. **Square** — Each of those differences is squared: `(predictions - labels) ** 2`.
3. **Sum** — NumPy sums the squared differences: `.sum()`.
4. **Divide** — Dividing by `n` gives the average — the error value for that set of predictions, and a score for the quality of the model.

This same pattern — vectorized subtraction, an element-wise operation, then an aggregation like `.sum()` or `.mean()` — is how most mathematical formulas translate directly into NumPy code without needing an explicit loop.


---

## 📝 Study Notes

- **This "subtract → transform → aggregate" pattern generalizes far beyond MSE.** Variance, standard deviation, dot products, cosine similarity, and most loss functions in machine learning follow the exact same shape: an element-wise operation followed by a `.sum()` or `.mean()`.
- **Vectorizing beats looping — often dramatically.** The same MSE calculation written with a Python `for` loop over elements can be 10-100x slower than the NumPy version above, because the loop runs in the (slow) Python interpreter instead of NumPy's compiled C code.
- **Watch your shapes before trusting a formula.** If `predictions` and `labels` don't have the same shape, NumPy will try to *broadcast* them instead of raising an error immediately — which can silently produce a meaningless result rather than a helpful crash. Always check `.shape` on both inputs first.

**Try it yourself:**
1. Implement Mean Absolute Error (MAE): the average of `|prediction - label|` instead of the squared difference. (Hint: `np.abs()`.)
2. Time the vectorized MSE against a pure-Python loop version on a 1-million-element array using `%timeit` (in a notebook) — how much faster is NumPy?
