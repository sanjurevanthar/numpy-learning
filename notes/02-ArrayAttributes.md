# Array Attributes

This section covers the `ndim`, `shape`, `size`, and `dtype` attributes of an array — plus a few related attributes that are useful to know.

```python
import numpy as np

a = np.array([[1, 2, 3, 4],
              [5, 6, 7, 8],
              [9, 10, 11, 12]])
```

## `ndim` — Number of Dimensions

The number of dimensions of an array is contained in the `ndim` attribute.

```python
a.ndim
# 2
```

## `shape` — Size Along Each Dimension

The shape of an array is a tuple of non-negative integers that specify the number of elements along each dimension.

```python
a.shape
# (3, 4)

len(a.shape) == a.ndim
# True
```

## `size` — Total Number of Elements

The fixed, total number of elements in the array is contained in the `size` attribute.

```python
a.size
# 12

import math
a.size == math.prod(a.shape)
# True
```

## `dtype` — Data Type

Arrays are typically "homogeneous," meaning that they contain elements of only one "data type." The data type is recorded in the `dtype` attribute.

```python
a.dtype
# dtype('int64')  # "int" for integer, "64" for 64-bit
```

Common `dtype` values you'll come across:

| dtype | Meaning |
|---|---|
| `int32` / `int64` | Signed integers (32-bit / 64-bit) |
| `float32` / `float64` | Floating-point numbers |
| `bool` | Boolean (`True`/`False`) |
| `complex128` | Complex numbers |
| `<U10`, `str_` | Unicode strings |

You can also set the `dtype` explicitly at creation time instead of letting NumPy infer it:

```python
np.array([1, 2, 3], dtype=np.float64).dtype
# dtype('float64')
```

## A Multi-Dimensional Example

These attributes generalize cleanly beyond 2D arrays. For example, with this 3D array:

```python
array_example = np.array([[[0, 1, 2, 3],
                            [4, 5, 6, 7]],

                           [[0, 1, 2, 3],
                            [4, 5, 6, 7]],

                           [[0, 1, 2, 3],
                            [4, 5, 6, 7]]])

array_example.ndim
# 3

array_example.size
# 24

array_example.shape
# (3, 2, 4)  -> 3 blocks, each with 2 rows and 4 columns
```

## Other Useful Attributes

A few additional attributes are commonly used alongside the four above:

```python
a.itemsize
# 8   -> size in bytes of one array element (int64 = 8 bytes)

a.nbytes
# 96  -> total bytes consumed: a.size * a.itemsize

a.T
# array([[1, 5, 9],
#        [2, 6, 10],
#        [3, 7, 11],
#        [4, 8, 12]])  -> transposed view of the array
```

## Quick Reference

| Attribute | Description | Example |
|---|---|---|
| `ndim` | Number of dimensions (axes) | `a.ndim` → `2` |
| `shape` | Tuple of elements per dimension | `a.shape` → `(3, 4)` |
| `size` | Total number of elements | `a.size` → `12` |
| `dtype` | Data type of the elements | `a.dtype` → `dtype('int64')` |
| `itemsize` | Bytes per element | `a.itemsize` → `8` |
| `nbytes` | Total bytes used by the array | `a.nbytes` → `96` |
| `T` | Transposed view of the array | `a.T` |

## Further Reading

- [Array attributes — NumPy documentation](https://numpy.org/doc/stable/reference/arrays.ndarray.html#arrays-ndarray)
- [Array objects — NumPy documentation](https://numpy.org/doc/stable/reference/arrays.html#arrays)

---

## 📝 Study Notes

- **`shape` vs `size` vs `ndim` — how they relate:** `size` is always the product of all numbers in `shape`, and `len(shape) == ndim`. If you remember this one relationship, you can derive any of the three from the other two in most problems.
- **`dtype` silently affects memory and math.** `int8` can only hold −128 to 127 — adding 1 to 127 wraps around (overflow) instead of raising an error in older NumPy versions. If your sums look wrong, check `.dtype` first.
- **A common exam-style trap:** `a.shape` returns a *tuple*, not a list — so `a.shape[0] = 5` will raise a `TypeError` since tuples are immutable. Use `a.reshape(...)` to actually change shape.

**Try it yourself:**
1. What is `a.itemsize * a.size` equal to? (Hint: check against `a.nbytes`.)
2. Create an array with `dtype=np.int8` and add 1 to the value 127. What happens?
