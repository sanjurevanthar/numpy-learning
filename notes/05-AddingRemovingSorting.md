# Adding, Removing, and Sorting Elements

This section covers `np.sort()`, `np.concatenate()`, and how to remove elements from an array.

## Sorting an Array

Sorting an array is simple with `np.sort()`. You can specify the `axis`, `kind`, and `order` when you call the function.

If you start with this array:

```python
import numpy as np

arr = np.array([2, 1, 5, 3, 7, 4, 6, 8])
```

You can quickly sort the numbers in ascending order with:

```python
np.sort(arr)
# array([1, 2, 3, 4, 5, 6, 7, 8])
```

`np.sort()` returns a **sorted copy** of the array — the original `arr` is left unchanged.

### Related Sorting Functions

In addition to `sort`, NumPy provides several other sorting-related functions:

| Function | Description |
|---|---|
| `np.argsort` | An indirect sort — returns the *indices* that would sort the array, along a specified axis. |
| `np.lexsort` | An indirect, stable sort on multiple keys. |
| `np.searchsorted` | Finds where elements should be inserted to keep a sorted array sorted. |
| `np.partition` | A partial sort — guarantees the k-th element is in its sorted position. |

```python
arr = np.array([2, 1, 5, 3, 7, 4, 6, 8])

np.argsort(arr)
# array([1, 0, 3, 5, 2, 6, 4, 7]) -> indices that would sort arr

np.searchsorted(np.sort(arr), 4)
# 3 -> index where 4 should be inserted to keep the array sorted
```

To read more about sorting an array, see the [NumPy sort documentation](https://numpy.org/doc/stable/reference/generated/numpy.sort.html).

## Concatenating Arrays

If you start with these arrays:

```python
a = np.array([1, 2, 3, 4])
b = np.array([5, 6, 7, 8])
```

You can concatenate them with `np.concatenate()`:

```python
np.concatenate((a, b))
# array([1, 2, 3, 4, 5, 6, 7, 8])
```

Or, if you start with these 2D arrays:

```python
x = np.array([[1, 2], [3, 4]])
y = np.array([[5, 6]])
```

You can concatenate them along a specific axis. `axis=0` stacks along rows (vertically):

```python
np.concatenate((x, y), axis=0)
# array([[1, 2],
#        [3, 4],
#        [5, 6]])
```

To concatenate along columns instead (horizontally), use `axis=1` — the arrays must have matching numbers of rows:

```python
x = np.array([[1, 2], [3, 4]])
z = np.array([[5], [6]])

np.concatenate((x, z), axis=1)
# array([[1, 2, 5],
#        [3, 4, 6]])
```

To read more about concatenate, see the [NumPy concatenate documentation](https://numpy.org/doc/stable/reference/generated/numpy.concatenate.html).

## Removing Elements

NumPy arrays have a fixed size, so there's no in-place "remove" operation. Instead, the simplest approach is to use indexing (or boolean masking) to select only the elements you want to **keep**, which returns a new array.

```python
arr = np.array([1, 2, 3, 4, 5, 6])

# Remove the element at index 2 (value 3)
new_arr = np.delete(arr, 2)
# array([1, 2, 4, 5, 6])

# Keep only elements greater than 3
filtered = arr[arr > 3]
# array([4, 5, 6])
```

`np.delete()` also returns a copy and accepts an `axis` argument for multi-dimensional arrays.

## See Also

- [Array Attributes](02-ArrayAttributes.md)
- [Indexing and Slicing](03-IndexingAndSlicing.md)

---

## 📝 Study Notes

- **`np.sort()` vs `arr.sort()`:** the function `np.sort(arr)` returns a *new* sorted copy and leaves `arr` untouched. The method `arr.sort()` sorts **in place** and returns `None`. Mixing these up is a very common bug — `arr = arr.sort()` will silently set `arr` to `None`.
- **`argsort` is more powerful than it looks.** Once you have the sorted *indices*, you can use them to reorder a second, related array the same way — e.g. sorting a list of names by a parallel array of scores: `names[scores.argsort()]`.
- **There is no `np.remove()`.** The idiomatic NumPy way to "remove" is almost always to build a boolean mask of what to *keep* (`arr[arr != value]`) rather than to think in terms of deleting from a fixed-size array.

**Try it yourself:**
1. You have `scores = np.array([88, 72, 95])` and `names = np.array(['Al', 'Bo', 'Cy'])`. Print the names sorted by score, highest first.
2. Remove all negative numbers from `np.array([3, -1, 4, -2, 5])` using boolean masking (not `np.delete`).
