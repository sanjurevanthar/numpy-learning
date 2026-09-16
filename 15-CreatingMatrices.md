# Creating Matrices

You can pass Python lists of lists to create a 2D array (or "matrix") to represent them in NumPy.

```python
import numpy as np

data = np.array([[1, 2], [3, 4], [5, 6]])
data
# array([[1, 2],
#        [3, 4],
#        [5, 6]])
```

## Indexing and Slicing Matrices

Indexing and slicing operations are useful when you're manipulating matrices:

```python
data[0, 1]
# 2

data[1:3]
# array([[3, 4],
#        [5, 6]])

data[0:2, 0]
# array([1, 3])
```

> For a full walkthrough of indexing and slicing, see [Indexing and Slicing](03-IndexingAndSlicing.md).

## Aggregating Matrices

You can aggregate matrices the same way you aggregated vectors:

```python
data.max()
# 6

data.min()
# 1

data.sum()
# 21
```

You can aggregate all the values in a matrix, and you can aggregate them across columns or rows using the `axis` parameter. To illustrate this point, let's look at a slightly modified dataset:

```python
data = np.array([[1, 2], [5, 3], [4, 6]])
data
# array([[1, 2],
#        [5, 3],
#        [4, 6]])

data.max(axis=0)
# array([5, 6])

data.max(axis=1)
# array([2, 5, 6])
```

> See [More Useful Array Operations](14-AggregationFunctions.md) for the full list of aggregation functions.

## Arithmetic on Matrices

Once you've created your matrices, you can add and multiply them using arithmetic operators if you have two matrices that are the same size.

```python
data = np.array([[1, 2], [3, 4]])
ones = np.array([[1, 1], [1, 1]])

data + ones
# array([[2, 3],
#        [4, 5]])
```

You can do these arithmetic operations on matrices of different sizes, but only if one matrix has only one column or one row. In this case, NumPy will use its broadcast rules for the operation:

```python
data = np.array([[1, 2], [3, 4], [5, 6]])
ones_row = np.array([[1, 1]])

data + ones_row
# array([[2, 3],
#        [4, 5],
#        [6, 7]])
```

> See [Broadcasting](13-Broadcasting.md) for the full rules on when this works.

## How NumPy Prints N-Dimensional Arrays

Be aware that when NumPy prints N-dimensional arrays, the last axis is looped over the fastest while the first axis is the slowest. For instance:

```python
np.ones((4, 3, 2))
# array([[[1., 1.],
#         [1., 1.],
#         [1., 1.]],
#
#        [[1., 1.],
#         [1., 1.],
#         [1., 1.]],
#
#        [[1., 1.],
#         [1., 1.],
#         [1., 1.]],
#
#        [[1., 1.],
#         [1., 1.],
#         [1., 1.]]])
```

## Initializing Arrays: `ones()`, `zeros()`, and Random Values

There are often instances where we want NumPy to initialize the values of an array. NumPy offers functions like `ones()` and `zeros()`, and the `random.Generator` class for random number generation. All you need to do is pass in the number of elements you want it to generate:

```python
np.ones(3)
# array([1., 1., 1.])

np.zeros(3)
# array([0., 0., 0.])

rng = np.random.default_rng()  # the simplest way to generate random numbers
rng.random(3)
# array([0.63696169, 0.26978671, 0.04097352])
```

You can also use `ones()`, `zeros()`, and `random()` to create a 2D array if you give them a tuple describing the dimensions of the matrix:

```python
np.ones((3, 2))
# array([[1., 1.],
#        [1., 1.],
#        [1., 1.]])

np.zeros((3, 2))
# array([[0., 0.],
#        [0., 0.],
#        [0., 0.]])

rng.random((3, 2))
# array([[0.01652764, 0.81327024],
#        [0.91275558, 0.60663578],
#        [0.72949656, 0.54362499]])  # may vary
```

Read more about creating arrays filled with 0's, 1's, other values, or uninitialized, in the [array creation routines documentation](https://numpy.org/doc/stable/reference/routines.array-creation.html).

## See Also

- [Random Number Generation](16-RandomNumbers.md)
- [Broadcasting](13-Broadcasting.md)
- [More Useful Array Operations](14-AggregationFunctions.md)

---

## 📝 Study Notes

- **`np.ones()` and `np.zeros()` default to `float64`**, even though you might expect integers — `np.ones(3)` gives `array([1., 1., 1.])`, not `array([1, 1, 1])`. Pass `dtype=int` explicitly if you need integers.
- **Print order (C-order) is worth internalizing** because it explains *why* `reshape` and `ravel`/`flatten` produce the results they do — NumPy always fills/reads the *last* axis fastest by default, so understanding this one rule demystifies several other topics in this guide.
- **`np.eye(n)`** (not covered above) is worth knowing alongside `ones`/`zeros` — it creates an `n x n` identity matrix, handy for linear algebra work.

**Try it yourself:**
1. Create a `3x3` matrix of all `7`s using `np.full()` (look up its signature) instead of multiplying `ones` by 7.
2. Predict, then verify, what `np.ones((2, 3), dtype=int)` prints compared to `np.ones((2, 3))`.
