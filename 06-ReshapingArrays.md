# Can You Reshape an Array?

This section covers `arr.reshape()`.

**Yes!**

Using `arr.reshape()` will return a reshaped array without changing the data. Just remember that when you use the reshape method, the array you want to produce needs to have the same number of elements as the original array. If you start with an array with 12 elements, you'll need to make sure that your new array also has a total of 12 elements.

## Basic Example

If you start with this array:

```python
import numpy as np

a = np.arange(6)
print(a)
# [0 1 2 3 4 5]
```

You can use `reshape()` to reshape your array. For example, you can reshape this array to an array with three rows and two columns:

```python
b = a.reshape(3, 2)
print(b)
# [[0 1]
#  [2 3]
#  [4 5]]
```

## Optional Parameters

With `np.reshape`, you can specify a few optional parameters:

```python
np.reshape(a, shape=(1, 6), order='C')
# array([[0, 1, 2, 3, 4, 5]])
```

- **`a`** — the array to be reshaped.
- **`shape`** — the new shape you want. You can specify an integer or a tuple of integers. If you specify an integer, the result will be an array of that length. The shape should be compatible with the original shape.
- **`order`** — `'C'` means to read/write the elements using C-like index order, `'F'` means to read/write the elements using Fortran-like index order, `'A'` means to read/write the elements in Fortran-like index order if `a` is Fortran contiguous in memory, C-like order otherwise. (This is an optional parameter and doesn't need to be specified.)

### C Order vs. Fortran Order

C and Fortran order have to do with how indices correspond to the order the array is stored in memory:

- **Fortran (column-major):** when moving through the elements of a two-dimensional array as it is stored in memory, the first index is the most rapidly varying index. As the first index moves to the next row, the matrix is stored one column at a time.
- **C (row-major):** the last index changes the most rapidly. The matrix is stored by rows.

Which one to use depends on whether it's more important to preserve the indexing convention or to keep the data from being reordered.

## Using `-1` to Infer a Dimension

You can pass `-1` for one dimension and NumPy will figure out the correct size automatically, based on the array's total number of elements:

```python
a = np.arange(6)

a.reshape(3, -1)
# array([[0, 1],
#        [2, 3],
#        [4, 5]])  -> NumPy infers 2 columns since 6 / 3 = 2
```

## A Common Pitfall: Mismatched Element Counts

Reshaping only works if the total number of elements stays the same. Trying to reshape into an incompatible shape raises an error:

```python
a = np.arange(6)  # 6 elements

a.reshape(4, 2)
# ValueError: cannot reshape array of size 6 into shape (4,2)
```

## Further Reading

- [Shape manipulation — NumPy documentation](https://numpy.org/doc/stable/reference/arrays.ndarray.html#reshaping-and-flattening-multidimensional-arrays)
- [Internal organization of NumPy arrays](https://numpy.org/doc/stable/dev/internals.html)

## See Also

- [Array Attributes](02-ArrayAttributes.md)
- [Adding a New Axis](09-AddingNewAxis.md)

---

## 📝 Study Notes

- **`reshape()` usually returns a view, not a copy** — if the data is contiguous in memory, reshaping doesn't move any data, it just changes how the same bytes are interpreted. This means modifying the reshaped array can modify the original, just like a slice.
- **`-1` can only appear once** in a `reshape()` call — NumPy needs at least one known dimension to calculate the unknown one. `a.reshape(-1, -1)` will raise an error.
- **A very common real-world use:** flattening a batch of images `(N, H, W, C)` into `(N, H*W*C)` before feeding it into a model that expects 2D input — `images.reshape(N, -1)` is the idiom.

**Try it yourself:**
1. Reshape `np.arange(12)` into shape `(2, -1)`. What shape does NumPy infer?
2. Try `np.arange(10).reshape(3, 3)`. What error do you get, and why?
