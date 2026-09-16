# Basic Array Operations

This section covers addition, subtraction, multiplication, division, and more.

## Arithmetic Operations

Once you've created your arrays, you can start to work with them. Let's say, for example, that you've created two arrays, one called `data` and one called `ones`:

```python
import numpy as np

data = np.array([1, 2])
ones = np.ones(2, dtype=np.int_)
```

You can add the arrays together with the plus sign:

```python
data + ones
# array([2, 3])
```

You can, of course, do more than just addition:

```python
data - ones
# array([0, 1])

data * data
# array([1, 4])

data / data
# array([1., 1.])
```

## Summing Elements

Basic operations are simple with NumPy. If you want to find the sum of the elements in an array, you'd use `sum()`. This works for 1D arrays, 2D arrays, and arrays in higher dimensions.

```python
a = np.array([1, 2, 3, 4])

a.sum()
# 10
```

## Summing Along an Axis

To add the rows or the columns in a 2D array, you would specify the axis.

If you start with this array:

```python
b = np.array([[1, 1], [2, 2]])
```

You can sum over the axis of rows with:

```python
b.sum(axis=0)
# array([3, 3])
```

You can sum over the axis of columns with:

```python
b.sum(axis=1)
# array([2, 4])
```

> **Tip:** `axis=0` operates "down the columns" (combining rows), and `axis=1` operates "across the rows" (combining columns). This convention holds across NumPy's aggregation functions — see [More Useful Array Operations](14-AggregationFunctions.md).

## See Also

- [Broadcasting](13-Broadcasting.md) — operating on arrays of different shapes.
- [More Useful Array Operations](14-AggregationFunctions.md) — `max`, `min`, `mean`, `std`, and more.
- [Creating Matrices](15-CreatingMatrices.md) — arithmetic on 2D arrays.

Learn more about basic operations in the [NumPy documentation](https://numpy.org/doc/stable/user/quickstart.html#basic-operations).

---

## 📝 Study Notes

- **Arithmetic operators are element-wise, not matrix operators.** `data * data` multiplies corresponding elements, it does *not* compute a matrix product. For actual matrix multiplication, use `@` or `np.matmul()` / `np.dot()`.
- **`axis=0` vs `axis=1` — a memory trick:** think of `axis=0` as walking *down* the rows (collapsing them into column totals), and `axis=1` as walking *across* the columns (collapsing them into row totals). The result's shape tells you which axis got "eaten."
- **Division by zero doesn't crash** in NumPy the way it does in plain Python — `np.array([1, 0]) / np.array([0, 0])` produces `[inf, nan]` with a runtime warning instead of raising an exception. Always sanity-check results that involve division.

**Try it yourself:**
1. For `b = np.array([[1, 2, 3], [4, 5, 6]])`, predict the shape of `b.sum(axis=0)` and `b.sum(axis=1)` before running the code.
2. Compute `data * data` and `data @ data` for `data = np.array([1, 2, 3])`. Why do they give different results (a vector vs. a scalar)?
