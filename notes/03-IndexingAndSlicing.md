# Indexing and Slicing

NumPy arrays are **0-indexed** (the first element is at index `0`) and are **mutable** — you can read and modify elements in place.

```python
import numpy as np

a = np.array([1, 2, 3, 4, 5, 6])

a[0]      # Accesses the first element (returns 1)
a[0] = 10 # Changes the first element to 10
```

## Negative Indexing

Negative indices count backward from the end of the array, so `-1` is the last element.

```python
a = np.array([10, 2, 3, 4, 5, 6])

a[-1]  # Returns 6 (the last element)
a[-2]  # Returns 5 (the second-to-last element)
```

## Indexing Multi-Dimensional Arrays

For multi-dimensional arrays, you can access elements using a single set of brackets with comma-separated indices `(row, column)` — instead of chaining brackets like `b[1][3]`.

```python
b = np.array([[1, 2, 3, 4],
              [5, 6, 7, 8],
              [9, 10, 11, 12]])

b[1, 3]   # Returns 8 (Element in row 1, column 3)
b[-1, -1] # Returns 12 (last row, last column)
```

## Slicing Basics

Slicing uses the syntax `start:stop:step` to pull out a sub-range of elements. `start` is inclusive, `stop` is exclusive, and `step` controls how many elements to skip.

```python
a = np.array([10, 2, 3, 4, 5, 6])

a[1:4]   # Returns array([2, 3, 4]) — index 1 up to (not including) index 4
a[:3]    # Returns array([10, 2, 3]) — from the start up to index 3
a[3:]    # Returns array([4, 5, 6]) — from index 3 to the end
a[::2]   # Returns array([10, 3, 5]) — every second element
a[::-1]  # Returns array([6, 5, 4, 3, 2, 10]) — reverses the array
```

### Slicing Multi-Dimensional Arrays

Each dimension can be sliced independently, separated by commas.

```python
b = np.array([[1, 2, 3, 4],
              [5, 6, 7, 8],
              [9, 10, 11, 12]])

b[0:2, 1:3]  # Rows 0–1, columns 1–2 -> array([[2, 3], [6, 7]])
b[:, 0]      # All rows, column 0    -> array([1, 5, 9])
b[1, :]      # Row 1, all columns    -> array([5, 6, 7, 8])
```

## ⚠️ Views vs. Copies

Unlike standard Python lists — where slicing creates a copy — slicing a NumPy array returns a **view** (a reference to the original data). Modifying a sliced view will modify the original array.

```python
a = np.array([10, 2, 3, 4, 5, 6])

c = a[:3] # Returns a view: array([10, 2, 3])
c[0] = 40 # This changes the first element in the original array 'a' to 40!

print(a)  # array([40, 2, 3, 4, 5, 6])
```

If you need an independent copy that won't affect the original array, use `.copy()` explicitly:

```python
c = a[:3].copy() # An independent copy, not a view
c[0] = 99         # 'a' is unaffected
```

> **Tip:** You can check whether an array owns its data or is a view into another array with `c.base is a` (returns `True` for a view) or `c.flags['OWNDATA']`.

## Boolean (Mask) Indexing

You can index an array with a boolean array of the same shape to select only the elements where the condition is `True`.

```python
a = np.array([10, 2, 3, 4, 5, 6])

mask = a > 4
a[mask]      # Returns array([10, 5, 6])
a[a > 4]     # Same thing, written inline
```

Boolean indexing always returns a **copy**, not a view.

## Fancy Indexing

You can pass a list or array of indices to select multiple specific, arbitrary elements at once.

```python
a = np.array([10, 2, 3, 4, 5, 6])

a[[0, 2, 4]]  # Returns array([10, 3, 5]) — elements at indices 0, 2, and 4
```

Like boolean indexing, fancy indexing always returns a **copy**, not a view.

## Quick Reference

| Technique | Example | Returns |
|---|---|---|
| Single index | `a[0]` | Copy of a single element |
| Basic slice | `a[1:4]` | **View** |
| Negative index | `a[-1]` | Copy of a single element |
| Multi-dim index | `b[1, 3]` | Copy of a single element |
| Multi-dim slice | `b[0:2, 1:3]` | **View** |
| Boolean mask | `a[a > 4]` | **Copy** |
| Fancy indexing | `a[[0, 2, 4]]` | **Copy** |

---

## 📝 Study Notes

- **The #1 NumPy bug for beginners:** forgetting that basic slices are views. If a function seems to be mysteriously changing your original data, check whether you passed a slice into it and whether the function modifies its input in place.
- **`stop` is always exclusive.** `a[1:4]` never includes index 4 — this matches Python list slicing exactly, so if you're comfortable with Python lists, NumPy 1D slicing works identically.
- **Multi-dimensional slices keep dimensions unless you index with a single integer.** `b[1, :]` returns a 1D array (dimension dropped), but `b[1:2, :]` returns a 2D array with one row (dimension kept). This distinction matters a lot when feeding data into ML models that expect a specific number of dimensions.

**Try it yourself:**
1. Given `b[1, :]` and `b[1:2, :]`, compare their `.shape`. Why are they different even though they "contain the same data"?
2. Write a slice that selects every other row and every other column of a 2D array.
