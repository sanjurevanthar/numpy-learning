# How to Reverse an Array

This section covers `np.flip()`.

NumPy's `np.flip()` function allows you to flip, or reverse, the contents of an array along an axis. When using `np.flip()`, specify the array you would like to reverse and the axis. If you don't specify the axis, NumPy will reverse the contents along *all* of the axes of your input array.

## Reversing a 1D Array

If you begin with a 1D array like this one:

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6, 7, 8])
```

You can reverse it with:

```python
reversed_arr = np.flip(arr)

print('Reversed Array: ', reversed_arr)
# Reversed Array:  [8 7 6 5 4 3 2 1]
```

## Reversing a 2D Array

A 2D array works much the same way.

If you start with this array:

```python
arr_2d = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
```

You can reverse the content in all of the rows and all of the columns with:

```python
reversed_arr = np.flip(arr_2d)
print(reversed_arr)
# [[12 11 10  9]
#  [ 8  7  6  5]
#  [ 4  3  2  1]]
```

You can easily reverse only the rows with:

```python
reversed_arr_rows = np.flip(arr_2d, axis=0)
print(reversed_arr_rows)
# [[ 9 10 11 12]
#  [ 5  6  7  8]
#  [ 1  2  3  4]]
```

Or reverse only the columns with:

```python
reversed_arr_columns = np.flip(arr_2d, axis=1)
print(reversed_arr_columns)
# [[ 4  3  2  1]
#  [ 8  7  6  5]
#  [12 11 10  9]]
```

## Reversing a Single Row or Column

You can also reverse the contents of only one column or row. For example, you can reverse the contents of the row at index position 1 (the second row):

```python
arr_2d[1] = np.flip(arr_2d[1])
print(arr_2d)
# [[ 1  2  3  4]
#  [ 8  7  6  5]
#  [ 9 10 11 12]]
```

You can also reverse the column at index position 1 (the second column):

```python
arr_2d[:, 1] = np.flip(arr_2d[:, 1])
print(arr_2d)
# [[ 1 10  3  4]
#  [ 8  7  6  5]
#  [ 9  2 11 12]]
```

> **Note:** Because indexing (`arr_2d[1]`, `arr_2d[:, 1]`) returns a view, assigning the flipped result back into that same slice updates the original array in place — see [Views vs. Copies](./indexing-and-slicing.md#️-views-vs-copies) for more on this behavior.

Read more about reversing arrays in the [`np.flip` documentation](https://numpy.org/doc/stable/reference/generated/numpy.flip.html).

## See Also

- [Indexing and Slicing](03-IndexingAndSlicing.md)

---

## 📝 Study Notes

- **`np.flip()` returns a view**, so it's cheap even on large arrays — but that also means modifying the flipped result can modify the original, the same caution as with basic slicing.
- **`arr[::-1]` is a common shorthand for `np.flip(arr)`** on a 1D array — both reverse the whole array, but `np.flip` is more explicit and easier to read when specifying an `axis` on multi-dimensional data.
- **Don't confuse flipping with transposing.** `np.flip(arr_2d, axis=0)` reverses the *order* of the rows; `arr_2d.T` swaps rows and columns entirely. They solve different problems.

**Try it yourself:**
1. Confirm `np.flip(arr)` and `arr[::-1]` give the same result for a 1D array.
2. For a 2D array, what's the difference between `np.flip(arr_2d)` (no axis) and `np.flip(arr_2d, axis=0)` followed by `np.flip(arr_2d, axis=1)`?
