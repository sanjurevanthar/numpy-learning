# Array Fundamentals

## What is an "Array"?

An array is a fundamental structure for storing and retrieving data, represented in NumPy by the `ndarray` (N-dimensional array) class.

You can visualize arrays by their number of dimensions:

- **1D array** — Like a standard list.
- **2D array** — Like a table (rows and columns).
- **3D array** — Like a stack of tables.

## Key Restrictions & Advantages

| Property | Description |
|---|---|
| **Homogeneous** | All elements must be of the exact same data type. |
| **Fixed Size** | Once created, the total size of the array cannot change. |
| **Rectangular** | The shape must be uniform (e.g., every row in a 2D array must have the same number of columns). |

> **Advantage:** By enforcing these restrictions, NumPy makes arrays significantly faster and more memory-efficient than standard Python data structures.

## Array Fundamentals

### Initialization

Arrays are easily initialized using standard Python sequences, such as lists:

```python
# 1D Array Initialization
a = np.array([1, 2, 3, 4, 5, 6])

# 2D Array Initialization (Nested Lists)
b = np.array([[1, 2, 3, 4],
              [5, 6, 7, 8],
              [9, 10, 11, 12]])
```

## Related Topics

### Core Basics

- [Indexing and Slicing](03-IndexingAndSlicing.md) — accessing, modifying, and slicing array elements, including views vs. copies, negative indexing, step slicing, and boolean/fancy indexing.
- [Array Attributes](02-ArrayAttributes.md) — `ndim`, `shape`, `size`, `dtype`, and other useful attributes like `itemsize`, `nbytes`, and `T`.
- [Creating Arrays from Existing Data](04-CreatingArraysFromExistingData.md) — slicing, `vstack`, `hstack`, `hsplit`, `.view()`, and `.copy()`.

### Shape and Structure

- [Adding, Removing, and Sorting Elements](05-AddingRemovingSorting.md) — `np.sort()`, `np.concatenate()`, and removing elements with indexing or `np.delete()`.
- [Reshaping Arrays](06-ReshapingArrays.md) — `arr.reshape()`, C vs. Fortran order, and inferring dimensions with `-1`.
- [Transposing a Matrix](07-TransposingArrays.md) — `arr.transpose()` and `arr.T`.
- [Reshaping and Flattening Arrays](08-FlatteningArrays.md) — `.flatten()` vs. `.ravel()`.
- [Adding a New Axis](09-AddingNewAxis.md) — converting 1D arrays to 2D with `np.newaxis` and `np.expand_dims`, plus `np.squeeze()`.
- [Reversing an Array](10-ReversingArrays.md) — `np.flip()` on 1D and 2D arrays.
- [Getting Unique Items and Counts](11-UniqueItemCount.md) — `np.unique()`, with indices and counts.

### Operations and Math

- [Basic Array Operations](12-BasicArrayOperations.md) — addition, subtraction, multiplication, division, and `sum()`.
- [Broadcasting](13-Broadcasting.md) — operating on arrays of different shapes.
- [More Useful Array Operations](14-AggregationFunctions.md) — `max`, `min`, `mean`, `std`, and more.
- [Creating Matrices](15-CreatingMatrices.md) — 2D arrays, matrix aggregation, arithmetic, and `ones()`/`zeros()`/`random()`.
- [Generating Random Numbers](16-RandomNumbers.md) — `np.random.default_rng()` and `Generator.integers`.
- [Working with Mathematical Formulas](17-MathematicalFormulas.md) — implementing formulas like mean squared error.

### Tooling and I/O

- [Accessing Docstrings](18-DocstringsAndHelp.md) — `help()`, `?`, and `??` in IPython.
- [Saving and Loading NumPy Objects](19-SaveLoadArrays.md) — `np.save`, `np.savez`, `np.savetxt`, `np.load`, `np.loadtxt`.
- [Importing and Exporting a CSV](20-ImportingExportingCSV.md) — reading and writing CSVs with Pandas.
- [Plotting Arrays with Matplotlib](21-PlottingWithMatplotlib.md) — line plots, `linspace`, and 3D surface plots.

---

## 📝 Study Notes

- **Why "homogeneous" matters:** because every element is the same type and size, NumPy can store the whole array as one contiguous block of memory and use fast, compiled C loops instead of Python's slower per-element loop. This is the entire reason NumPy is fast.
- **`np.array()` picks a dtype for you.** If you mix an int and a float in the same list, NumPy silently upcasts everything to `float64`. `np.array([1, 2, 3.0])` → `dtype('float64')`. Check `.dtype` if a result looks unexpectedly like `1.0` instead of `1`.
- **"Fixed size" trips people up coming from lists.** There's no `.append()` that grows an array in place — operations that look like they add elements (e.g. `np.append`, `np.concatenate`) actually build a brand-new array behind the scenes. For large loops, this is slow; prefer building a Python list first and converting to an array once at the end.

**Try it yourself:**
1. Create a 2D array from a nested list where one row is missing a value — what error do you get, and why?
2. Create `np.array([1, 2, 3])` and `np.array([1.0, 2, 3])`. Compare their `.dtype`. Why do they differ?
