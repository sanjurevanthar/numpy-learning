# How to Create an Array from Existing Data

This section covers slicing and indexing, `np.vstack()`, `np.hstack()`, `np.hsplit()`, `.view()`, and `.copy()`.

## Slicing to Create a New Array

You can easily create a new array from a section of an existing array.

Let's say you have this array:

```python
import numpy as np

a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
```

You can create a new array from a section of your array any time by specifying where you want to slice your array:

```python
arr1 = a[3:8]
arr1
# array([4, 5, 6, 7, 8])
```

Here, you grabbed a section of your array from index position 3 through index position 8, but not including position 8 itself.

> **Reminder:** Array indexes begin at 0. This means the first element of the array is at index 0, the second element is at index 1, and so on.

## Stacking Arrays

You can also stack two existing arrays, both vertically and horizontally.

Let's say you have two arrays, `a1` and `a2`:

```python
a1 = np.array([[1, 1],
               [2, 2]])

a2 = np.array([[3, 3],
               [4, 4]])
```

You can stack them vertically with `vstack`:

```python
np.vstack((a1, a2))
# array([[1, 1],
#        [2, 2],
#        [3, 3],
#        [4, 4]])
```

Or stack them horizontally with `hstack`:

```python
np.hstack((a1, a2))
# array([[1, 1, 3, 3],
#        [2, 2, 4, 4]])
```

## Splitting Arrays with `hsplit`

You can split an array into several smaller arrays using `hsplit`. You can specify either the number of equally shaped arrays to return, or the columns after which the division should occur.

Let's say you have this array:

```python
x = np.arange(1, 25).reshape(2, 12)
x
# array([[ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12],
#        [13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24]])
```

If you wanted to split this array into three equally shaped arrays, you would run:

```python
np.hsplit(x, 3)
# [array([[ 1,  2,  3,  4], [13, 14, 15, 16]]),
#  array([[ 5,  6,  7,  8], [17, 18, 19, 20]]),
#  array([[ 9, 10, 11, 12], [21, 22, 23, 24]])]
```

If you wanted to split your array after the third and fourth column, you'd run:

```python
np.hsplit(x, (3, 4))
# [array([[ 1,  2,  3], [13, 14, 15]]),
#  array([[ 4], [16]]),
#  array([[ 5,  6,  7,  8,  9, 10, 11, 12], [17, 18, 19, 20, 21, 22, 23, 24]])]
```

`np.vsplit()` works the same way but splits along rows instead of columns.

Learn more about stacking and splitting arrays in the [NumPy documentation](https://numpy.org/doc/stable/reference/routines.array-manipulation.html).

## `.view()` — Shallow Copies

You can use the `view` method to create a new array object that looks at the same data as the original array (a **shallow copy**).

Views are an important NumPy concept. NumPy functions, as well as operations like indexing and slicing, will return views whenever possible. This saves memory and is faster (no copy of the data has to be made). However, it's important to be aware of this: **modifying data in a view also modifies the original array!**

Let's say you create this array:

```python
a = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
```

Now create an array `b1` by slicing `a` and modify the first element of `b1`. This will modify the corresponding element in `a` as well:

```python
b1 = a[0, :]
b1
# array([1, 2, 3, 4])

b1[0] = 99
b1
# array([99,  2,  3,  4])

a
# array([[99,  2,  3,  4],
#        [ 5,  6,  7,  8],
#        [ 9, 10, 11, 12]])
```

## `.copy()` — Deep Copies

Using the `copy` method will make a complete copy of the array and its data (a **deep copy**), independent of the original. To use this on your array, you could run:

```python
b2 = a.copy()
```

Changes to `b2` will *not* affect `a`.

> For more on the distinction between views and copies, see [Views vs. Copies](./indexing-and-slicing.md#️-views-vs-copies).

## See Also

- [Indexing and Slicing](03-IndexingAndSlicing.md)
- [Reshaping Arrays](06-ReshapingArrays.md)

---

## 📝 Study Notes

- **`vstack`/`hstack` require compatible shapes.** `vstack` needs matching column counts; `hstack` needs matching row counts. If you get a `ValueError: all the input array dimensions... must match`, print both arrays' `.shape` first.
- **`view()` vs a basic slice:** a basic slice (`a[1:3]`) is already a view — calling `.view()` explicitly is mostly useful when you want a view with a *different dtype interpretation* of the same underlying bytes (an advanced use case), or want to be explicit in code that a copy is *not* being made.
- **When in doubt, copy.** If you're not 100% sure whether an operation returns a view or a copy, and correctness matters more than the small memory/speed savings, just call `.copy()` explicitly. It costs little for small-to-medium arrays and eliminates an entire class of bugs.

**Try it yourself:**
1. Split a `4x12` array into 4 equal pieces with `hsplit`. What's the shape of each piece?
2. Create array `a`, take `b = a.view()`, modify `b[0]`, and check `a[0]`. Then repeat with `b = a.copy()`. Compare.
