# Reshaping and Flattening Multidimensional Arrays

This section covers `.flatten()` and `.ravel()`.

There are two popular ways to flatten an array: `.flatten()` and `.ravel()`. The primary difference between the two is that the new array created using `ravel()` is actually a **reference** to the parent array (i.e., a "view"). This means that any changes to the new array will affect the parent array as well. Since `ravel` does not create a copy, it's memory efficient.

## `.flatten()` — Returns a Copy

If you start with this array:

```python
import numpy as np

x = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
```

You can use `flatten` to flatten your array into a 1D array:

```python
x.flatten()
# array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12])
```

When you use `flatten`, changes to your new array won't change the parent array. For example:

```python
a1 = x.flatten()
a1[0] = 99

print(x)  # Original array
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]

print(a1)  # New array
# [99  2  3  4  5  6  7  8  9 10 11 12]
```

## `.ravel()` — Returns a View

But when you use `ravel`, the changes you make to the new array *will* affect the parent array. For example:

```python
a2 = x.ravel()
a2[0] = 98

print(x)  # Original array
# [[98  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]

print(a2)  # New array
# [98  2  3  4  5  6  7  8  9 10 11 12]
```

## `flatten()` vs. `ravel()`

| | `.flatten()` | `.ravel()` |
|---|---|---|
| Returns | Copy | View (when possible) |
| Modifying result affects original? | No | Yes |
| Memory | Uses more (allocates new data) | More memory efficient |
| When to use | You need an independent 1D array | You just need to read/iterate, or don't mind shared memory |

Read more about `flatten` at [`ndarray.flatten`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.flatten.html) and `ravel` at [`ravel`](https://numpy.org/doc/stable/reference/generated/numpy.ravel.html).

## See Also

- [Reshaping Arrays](06-ReshapingArrays.md)
- [Indexing and Slicing](03-IndexingAndSlicing.md) — more on views vs. copies.

---

## 📝 Study Notes

- **Quick rule of thumb:** if you're just *reading* the data (e.g. iterating, passing to a function that doesn't modify it), prefer `ravel()` for its speed and lower memory use. If you plan to *modify* the flattened result independently of the original, use `flatten()`.
- **`ravel()` isn't *always* a view.** If the array isn't contiguous in memory (for example, after some kinds of slicing or transposing), `ravel()` has to make a copy anyway to produce a flat 1D result — you can check `np.shares_memory(a, a.ravel())` to confirm.
- **Both produce C-order (row-major) results by default** — meaning they read row by row. Pass `order='F'` to read column by column instead, matching Fortran-style memory layout.

**Try it yourself:**
1. Flatten a `(3, 4)` array with both `.flatten()` and `.ravel()`, modify index `0` of each result, and check which one changed the original array.
2. Try `arr.T.ravel()` — is the result a view or a copy? (Hint: transpose changes contiguity.)
