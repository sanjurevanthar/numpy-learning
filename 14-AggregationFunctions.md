# More Useful Array Operations

This section covers `maximum`, `minimum`, `sum`, `mean`, `product`, `standard deviation`, and more.

NumPy performs aggregation functions across arrays. In addition to `min`, `max`, and `sum`, you can easily run `mean` to get the average, `prod` to get the result of multiplying the elements together, `std` to get the standard deviation, and more.

```python
import numpy as np

data = np.array([1, 2, 3])

data.max()
# 3

data.min()
# 1

data.sum()
# 6
```

## Aggregating Along an Axis

Let's start with this array, called `a`:

```python
a = np.array([[0.45053314, 0.17296777, 0.34376245, 0.5510652],
              [0.54627315, 0.05093587, 0.40067661, 0.55645993],
              [0.12697628, 0.82485143, 0.26590556, 0.56917101]])
```

It's very common to want to aggregate along a row or column. By default, every NumPy aggregation function will return the aggregate of the entire array. To find the sum or the minimum of the elements in your array, run:

```python
a.sum()
# 4.8595784
```

Or:

```python
a.min()
# 0.05093587
```

You can specify on which axis you want the aggregation function to be computed. For example, you can find the minimum value within each column by specifying `axis=0`:

```python
a.min(axis=0)
# array([0.12697628, 0.05093587, 0.26590556, 0.5510652 ])
```

The four values listed above correspond to the number of columns in your array. With a four-column array, you will get four values as your result.

## Common Aggregation Functions

| Function | Description |
|---|---|
| `arr.sum()` | Sum of all elements |
| `arr.min()` / `arr.max()` | Minimum / maximum element |
| `arr.mean()` | Average of all elements |
| `arr.prod()` | Product of all elements (multiplied together) |
| `arr.std()` | Standard deviation |
| `arr.var()` | Variance |
| `arr.argmin()` / `arr.argmax()` | Index of the minimum / maximum element |

All of these accept an `axis` parameter, just like `sum()` and `min()` above.

Read more about array methods in the [NumPy documentation](https://numpy.org/doc/stable/reference/arrays.ndarray.html#calculation).

## See Also

- [Basic Array Operations](12-BasicArrayOperations.md)
- [Creating Matrices](15-CreatingMatrices.md)

---

## 📝 Study Notes

- **`mean()` vs `sum() / size` — they're the same, but `.mean()` is more readable and less error-prone**, especially with an `axis` argument where manually dividing by the right count gets fiddly.
- **NaN-safe variants exist for every aggregation.** If your data has missing values represented as `np.nan`, regular `.sum()`/`.mean()` will return `nan` for the whole result. Use `np.nansum()`, `np.nanmean()`, `np.nanstd()`, etc. instead.
- **`argmin`/`argmax` return the *index*, not the value.** A common mistake is expecting `arr.argmax()` to return the maximum value itself — pair it with indexing (`arr[arr.argmax()]`) to get the value.

**Try it yourself:**
1. For `a = np.array([3.0, np.nan, 7.0])`, compare `a.mean()` and `np.nanmean(a)`.
2. Find the index of the largest value in each row of a 2D array using `argmax` with the correct `axis`.
