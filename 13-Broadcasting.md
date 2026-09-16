# Broadcasting

There are times when you might want to carry out an operation between an array and a single number (also called an operation between a vector and a scalar), or between arrays of two different sizes.

For example, your array (we'll call it `data`) might contain information about distance in miles, but you want to convert the information to kilometers. You can perform this operation with:

```python
import numpy as np

data = np.array([1.0, 2.0])
data * 1.6
# array([1.6, 3.2])
```

NumPy understands that the multiplication should happen with each cell — that concept is called **broadcasting**.

## What Is Broadcasting?

Broadcasting is a mechanism that allows NumPy to perform operations on arrays of different shapes. The dimensions of your array must be **compatible**, for example, when the dimensions of both arrays are equal, or when one of them is `1`. If the dimensions are not compatible, you will get a `ValueError`.

## Broadcasting Rules

When comparing the shapes of two arrays element-wise (starting from the trailing/rightmost dimension), two dimensions are compatible when:

1. They are equal, or
2. One of them is `1`.

If neither condition holds for any dimension pair, NumPy raises a `ValueError: operands could not be broadcast together`.

## More Examples

Broadcasting a scalar against a 2D array:

```python
data = np.array([[1, 2], [3, 4]])
data * 10
# array([[10, 20],
#        [30, 40]])
```

Broadcasting a 1D array against a 2D array (matching the number of columns):

```python
data = np.array([[1, 2], [3, 4], [5, 6]])   # shape (3, 2)
ones_row = np.array([[1, 1]])               # shape (1, 2)

data + ones_row
# array([[2, 3],
#        [4, 5],
#        [6, 7]])
```

Here, `ones_row`'s shape `(1, 2)` is compatible with `data`'s shape `(3, 2)` because the second dimension matches (`2 == 2`) and the first dimension is `1`, so it's stretched across all 3 rows.

## An Incompatible Example

```python
a = np.array([1, 2, 3])        # shape (3,)
b = np.array([1, 2])           # shape (2,)

a + b
# ValueError: operands could not be broadcast together with shapes (3,) (2,)
```

Learn more about broadcasting in the [NumPy documentation](https://numpy.org/doc/stable/user/basics.broadcasting.html).

## See Also

- [Basic Array Operations](12-BasicArrayOperations.md)
- [Creating Matrices](15-CreatingMatrices.md)

---

## 📝 Study Notes

- **Compare shapes right-to-left, not left-to-right.** NumPy aligns the *trailing* dimensions first. A `(3, 4)` array and a `(4,)` array broadcast fine (the `4` matches), but a `(3, 4)` array and a `(3,)` array do not, even though `3` "matches" the first dimension — you'd need to reshape the `(3,)` array to `(3, 1)` first.
- **Broadcasting never actually copies data to "pad" the smaller array.** It's a virtual, memory-efficient stretch — no matter how large the resulting shape is, the smaller array's data is only stored once.
- **This is the single most useful NumPy concept to internalize deeply** — nearly every "why is my shape wrong" error traces back to a misunderstanding of broadcasting rules.

**Try it yourself:**
1. Will `np.zeros((3, 4)) + np.zeros((4,))` broadcast successfully? What about `np.zeros((3, 4)) + np.zeros((3,))`?
2. What shape do you need to reshape a `(3,)` array to, so it broadcasts against a `(3, 4)` array by matching the *rows* instead of the columns?
