# Matrix Multiplication

This section covers `@`, `np.matmul()`, and `np.dot()` — and how matrix multiplication differs from the element-wise arithmetic covered in [Basic Array Operations](12-BasicArrayOperations.md).

## Element-wise vs. Matrix Multiplication

This is one of the most common points of confusion for people coming to NumPy from regular Python. The `*` operator multiplies **element-wise** — each element is multiplied with the element in the same position:

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

a * b
# array([ 4, 10, 18])  -> element-wise: [1*4, 2*5, 3*6]
```

True **matrix multiplication** (the dot product / linear-algebra sense) is a different operation entirely, and NumPy gives you a dedicated operator for it: `@`.

```python
a @ b
# 32  -> (1*4) + (2*5) + (3*6) = 4 + 10 + 18 = 32
```

For two 1D arrays, `@` computes the **dot product**: multiply corresponding elements, then sum them all into a single number.

## `@` on 2D Arrays (Matrices)

For 2D arrays, `@` performs standard matrix multiplication — each element of the result is the dot product of a row from the first matrix and a column from the second.

```python
A = np.array([[1, 2],
              [3, 4]])

B = np.array([[5, 6],
              [7, 8]])

A @ B
# array([[19, 22],
#        [43, 50]])
```

Compare that to element-wise multiplication of the same two matrices:

```python
A * B
# array([[ 5, 12],
#        [21, 32]])
```

These are two completely different results from two completely different operations — it's worth running both side by side until the distinction feels automatic.

## The Shape Rule

For `A @ B` to work, the **inner dimensions must match**: if `A` has shape `(m, n)`, `B` must have shape `(n, p)`, and the result has shape `(m, p)`.

```python
A = np.ones((2, 3))  # shape (2, 3)
B = np.ones((3, 4))  # shape (3, 4)

(A @ B).shape
# (2, 4)  -> the "3"s cancel out, leaving (2, 4)
```

If the inner dimensions don't match, you'll get a clear error:

```python
A = np.ones((2, 3))
B = np.ones((2, 4))

A @ B
# ValueError: matmul: Input operand 1 has a mismatch in its core dimension 0...
```

> **Tip:** A quick way to remember the rule — write the two shapes next to each other, `(m, n) @ (n, p)`. The two middle numbers must match; they disappear, and the two outer numbers become the result's shape.

## `@` vs. `np.matmul()` vs. `np.dot()`

All three can perform matrix multiplication, and for plain 2D matrices they behave identically:

```python
A @ B
np.matmul(A, B)
np.dot(A, B)
# All three give the same result for 2D arrays
```

| | `@` | `np.matmul()` | `np.dot()` |
|---|---|---|---|
| 2D matrix multiplication | ✅ | ✅ | ✅ |
| Readability | Most readable (operator form) | Explicit function form | Explicit function form |
| 1D arrays (vectors) | Dot product | Dot product | Dot product |
| Scalar multiplication | ❌ Not supported | ❌ Not supported | ✅ Falls back to element-wise |
| Arrays with more than 2 dimensions | Broadcasts as a "stack" of matrix multiplications | Same as `@` | Different behavior — sums over the last axis of the first array and the second-to-last of the second, which is rarely what you want for stacked/batched matrices |

**In practice:** use `@` (or `np.matmul()`, which `@` calls under the hood) for anything 2D or "batch of matrices" — it's the modern, recommended choice. Reach for `np.dot()` mainly when you specifically need its legacy scalar-multiplication behavior, or when maintaining older code that already uses it.

## A Worked Example: Linear Regression Predictions

Matrix multiplication is everywhere in real-world NumPy code — for example, computing predictions from a linear model given a feature matrix `X` and a weight vector `w`:

```python
X = np.array([[1, 2],
              [3, 4],
              [5, 6]])   # 3 samples, 2 features each

w = np.array([0.5, 1.5])  # one weight per feature

predictions = X @ w
# array([ 3.5,  7.5, 11.5])
```

Here, `X` has shape `(3, 2)` and `w` has shape `(2,)` — NumPy treats the 1D `w` as a column vector for the purposes of the multiplication, giving a `(3,)` result: one prediction per sample.

## `.T` and `@` Together

Transposing and matrix multiplication are often used together — for example, computing `XᵀX`, a common step in linear algebra and statistics:

```python
X = np.array([[1, 2],
              [3, 4],
              [5, 6]])

X.T @ X
# array([[35, 44],
#        [44, 56]])
```

> See [Transposing a Matrix](07-TransposingArrays.md) for more on `.T`.

Read more about matrix multiplication in the [`np.matmul` documentation](https://numpy.org/doc/stable/reference/generated/numpy.matmul.html) and [`np.dot` documentation](https://numpy.org/doc/stable/reference/generated/numpy.dot.html).

---

## 📝 Study Notes

- **The single most important habit:** before writing `A @ B`, check `A.shape` and `B.shape` and mentally confirm the inner dimensions match. This one check prevents the majority of matrix-multiplication bugs.
- **`*` and `@` are *not* interchangeable**, even though it's tempting to think of `@` as "just another multiply." Mixing them up silently produces a valid but *wrong* array (rather than an error) whenever the shapes happen to be compatible with both operations — which makes this bug particularly sneaky to catch.
- **For 1D arrays, `@` collapses to a single number** (the dot product), not an array — this trips people up when they expect an array back and get a scalar instead.
- **`(A @ B) is not (B @ A)` in general.** Unlike regular multiplication of numbers, matrix multiplication is not commutative — order matters, and the shapes may not even both be valid in reverse order.

**Try it yourself:**
1. For `A` of shape `(4, 3)` and `B` of shape `(3, 5)`, what shape is `A @ B`? What shape (if any) is `B @ A`?
2. Compute both `A * B` and `A @ B` for two `2x2` matrices of your choice, and confirm by hand that the `@` result matches the row-times-column dot product definition.
3. Using the linear regression example above, add a third weight and a third feature column to `X` and `w`, and recompute the predictions.

## See Also

- [Basic Array Operations](12-BasicArrayOperations.md) — element-wise arithmetic.
- [Transposing a Matrix](07-TransposingArrays.md) — `.T`, often paired with `@`.
- [Broadcasting](13-Broadcasting.md) — the rules that govern element-wise operations between differently-shaped arrays (a separate mechanism from matrix multiplication's shape rule).
