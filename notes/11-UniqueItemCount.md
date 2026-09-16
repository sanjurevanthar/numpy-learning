# How to Get Unique Items and Counts

This section covers `np.unique()`.

You can find the unique elements in an array easily with `np.unique`.

## Basic Usage

For example, if you start with this array:

```python
import numpy as np

a = np.array([11, 11, 12, 13, 14, 15, 16, 17, 12, 13, 11, 14, 18, 19, 20])
```

You can use `np.unique` to print the unique values in your array:

```python
unique_values = np.unique(a)
print(unique_values)
# [11 12 13 14 15 16 17 18 19 20]
```

## Getting the Index of Each Unique Value

To get the indices of unique values in a NumPy array (an array of first index positions of unique values in the array), pass the `return_index` argument along with your array:

```python
unique_values, indices_list = np.unique(a, return_index=True)
print(indices_list)
# [ 0  2  3  4  5  6  7 12 13 14]
```

## Getting the Frequency Count of Each Unique Value

You can pass the `return_counts` argument along with your array to get the frequency count of unique values:

```python
unique_values, occurrence_count = np.unique(a, return_counts=True)
print(occurrence_count)
# [3 2 2 2 1 1 1 1 1 1]
```

## Working with 2D Arrays

This also works with 2D arrays. If you start with this array:

```python
a_2d = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [1, 2, 3, 4]])
```

You can find unique values with:

```python
unique_values = np.unique(a_2d)
print(unique_values)
# [ 1  2  3  4  5  6  7  8  9 10 11 12]
```

> If the `axis` argument isn't passed, your 2D array will be flattened first.

If you want to get the unique rows or columns, make sure to pass the `axis` argument. To find the unique rows, specify `axis=0`; for columns, specify `axis=1`:

```python
unique_rows = np.unique(a_2d, axis=0)
print(unique_rows)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
```

## Combining `axis`, `return_index`, and `return_counts`

To get the unique rows, their index positions, and occurrence counts all at once, you can use:

```python
unique_rows, indices, occurrence_count = np.unique(
    a_2d, axis=0, return_counts=True, return_index=True)

print(unique_rows)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]

print(indices)
# [0 1 2]

print(occurrence_count)
# [2 1 1]
```

To learn more about finding the unique elements in an array, see the [`np.unique` documentation](https://numpy.org/doc/stable/reference/generated/numpy.unique.html).

---

## 📝 Study Notes

- **`np.unique()` always sorts its output.** Even if `11` appears before `5` in your original array, the unique values come back in ascending order — if you need unique values in their *original* order of first appearance, sort the returned `indices_list` and use it to reindex.
- **A handy one-liner combo:** `values, counts = np.unique(a, return_counts=True)` followed by `values[counts.argmax()]` gives you the most frequent element in an array — a common interview-style question.
- **On 2D arrays, always think about `axis` first.** Forgetting `axis=0` is the most common mistake here — without it, NumPy flattens the array before finding uniques, which is rarely what you want when you're really asking "which rows are duplicates?"

**Try it yourself:**
1. Find the most frequently occurring value in `np.array([4, 4, 2, 2, 2, 7])` using `np.unique` with `return_counts=True`.
2. Given a 2D array with some duplicate rows, find just the duplicated rows (hint: look at `return_counts` combined with `axis=0`).
