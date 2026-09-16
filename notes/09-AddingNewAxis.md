# How to Convert a 1D Array into a 2D Array (Adding a New Axis)

This section covers `np.newaxis` and `np.expand_dims`.

You can use `np.newaxis` and `np.expand_dims` to increase the dimensions of your existing array.

## `np.newaxis`

Using `np.newaxis` will increase the dimensions of your array by one dimension when used once. This means that a 1D array will become a 2D array, a 2D array will become a 3D array, and so on.

For example, if you start with this array:

```python
import numpy as np

a = np.array([1, 2, 3, 4, 5, 6])
a.shape
# (6,)
```

You can use `np.newaxis` to add a new axis:

```python
a2 = a[np.newaxis, :]
a2.shape
# (1, 6)
```

### Row Vectors vs. Column Vectors

You can explicitly convert a 1D array to either a row vector or a column vector using `np.newaxis`.

Convert to a **row vector** by inserting an axis along the first dimension:

```python
row_vector = a[np.newaxis, :]
row_vector.shape
# (1, 6)
```

Or, for a **column vector**, insert an axis along the second dimension:

```python
col_vector = a[:, np.newaxis]
col_vector.shape
# (6, 1)
```

## `np.expand_dims`

You can also expand an array by inserting a new axis at a specified position with `np.expand_dims`.

For example, if you start with this array:

```python
a = np.array([1, 2, 3, 4, 5, 6])
a.shape
# (6,)
```

You can use `np.expand_dims` to add an axis at index position 1 with:

```python
b = np.expand_dims(a, axis=1)
b.shape
# (6, 1)
```

You can add an axis at index position 0 with:

```python
c = np.expand_dims(a, axis=0)
c.shape
# (1, 6)
```

## `np.newaxis` vs. `np.expand_dims`

Both approaches produce the same result — the choice mostly comes down to style and context:

| Approach | Style | Best for |
|---|---|---|
| `a[np.newaxis, :]` | Indexing-based | Quick, inline axis insertion while slicing |
| `np.expand_dims(a, axis=n)` | Function-based | Clearer intent in code, especially when the axis is computed dynamically |

## Removing a Size-1 Axis: `np.squeeze`

The inverse operation — removing axes of length 1 — is done with `np.squeeze()`:

```python
b = np.expand_dims(a, axis=1)  # shape (6, 1)

np.squeeze(b).shape
# (6,)  -> back to the original 1D shape
```

## Further Reading

- [`np.newaxis` documentation](https://numpy.org/doc/stable/reference/constants.html#numpy.newaxis)
- [`np.expand_dims` documentation](https://numpy.org/doc/stable/reference/generated/numpy.expand_dims.html)

## See Also

- [Array Attributes](02-ArrayAttributes.md)
- [Reshaping Arrays](06-ReshapingArrays.md)

---

## 📝 Study Notes

- **Why you'd ever want a size-1 axis:** it's almost always for [broadcasting](13-Broadcasting.md). A shape `(6,)` array can't be broadcast against a shape `(6, 3)` array directly, but `(6, 1)` can — inserting that extra axis is what makes many "vector vs. matrix" operations work without writing a loop.
- **`None` is a shorthand for `np.newaxis`.** You'll often see `a[:, None]` in other people's code — it does exactly the same thing as `a[:, np.newaxis]`, just more tersely.
- **`expand_dims` with `axis=-1`** is a very common pattern for adding a trailing "channel" dimension (e.g. turning a grayscale image `(H, W)` into `(H, W, 1)` to match a model's expected input shape).

**Try it yourself:**
1. Turn a 1D array of shape `(5,)` into shape `(5, 1, 1)` using two applications of `np.newaxis` or `expand_dims`.
2. Use `np.squeeze()` to reverse it back to `(5,)` in one call.
