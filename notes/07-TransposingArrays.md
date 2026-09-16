# Transposing a Matrix

This section covers `arr.transpose()` and `arr.T`. (For reshaping with `arr.reshape()`, see [Reshaping Arrays](06-ReshapingArrays.md).)

It's common to need to transpose your matrices. NumPy arrays have the property `T` that allows you to transpose a matrix.

## Reshaping vs. Transposing

You may also need to switch the dimensions of a matrix — this can happen when, for example, you have a model that expects a certain input shape that is different from your dataset. This is where the `reshape` method can be useful:

```python
import numpy as np

data = np.array([[1, 2, 3], [4, 5, 6]])

data.reshape(2, 3)
# array([[1, 2, 3],
#        [4, 5, 6]])

data.reshape(3, 2)
# array([[1, 2],
#        [3, 4],
#        [5, 6]])
```

Transposing is a *related but different* operation — reshape reinterprets the same data in a new shape, while transpose actually flips the array across its diagonal, swapping rows and columns.

## Using `.transpose()`

You can use `.transpose()` to reverse or change the axes of an array according to the values you specify.

If you start with this array:

```python
arr = np.arange(6).reshape((2, 3))
arr
# array([[0, 1, 2],
#        [3, 4, 5]])
```

You can transpose your array with `arr.transpose()`:

```python
arr.transpose()
# array([[0, 3],
#        [1, 4],
#        [2, 5]])
```

## Using `.T`

You can also use the shorthand `arr.T`:

```python
arr.T
# array([[0, 3],
#        [1, 4],
#        [2, 5]])
```

`.T` is simply a convenient shortcut for `.transpose()` with no arguments — for a 2D array, this swaps rows and columns.

To learn more about transposing and reshaping arrays, see [`transpose`](https://numpy.org/doc/stable/reference/generated/numpy.transpose.html) and [`reshape`](https://numpy.org/doc/stable/reference/generated/numpy.reshape.html) in the NumPy documentation.

## See Also

- [Reshaping Arrays](06-ReshapingArrays.md)
- [Array Attributes](02-ArrayAttributes.md) — `.T` is listed here too as a quick attribute reference.

---

## 📝 Study Notes

- **Transpose is a view, not a copy** — just like a basic slice. `arr.T` doesn't rearrange any data in memory; it just changes the "strides" (the step sizes used to walk through memory), so it's essentially free no matter how large the array is.
- **1D arrays don't actually transpose.** `np.array([1, 2, 3]).T` returns the exact same 1D array — there's no second axis to flip. To get a true row/column vector, use [`np.newaxis`](09-AddingNewAxis.md) first, then transpose.
- **For arrays with more than 2 dimensions**, `.T` reverses *all* axes by default — if you only want to swap two specific axes, use `np.swapaxes(arr, axis1, axis2)` or pass an explicit axis order to `.transpose(order)`.

**Try it yourself:**
1. Confirm that `np.array([1, 2, 3]).T.shape` is the same as the original. Then compare it to `np.array([1, 2, 3])[:, np.newaxis].T.shape`.
2. For a 3D array of shape `(2, 3, 4)`, what is the shape after `.T`?
