# How to Access the Docstring for More Information

This section covers `help()`, `?`, and `??`.

When it comes to the data science ecosystem, Python and NumPy are built with the user in mind. One of the best examples of this is the built-in access to documentation. Every object contains a reference to a string known as the **docstring**. In most cases, this docstring contains a quick and concise summary of the object and how to use it.

## Using `help()`

Python has a built-in `help()` function that can help you access this information. This means that nearly any time you need more information, you can use `help()` to quickly find what you need.

For example:

```python
help(max)
```
```
Help on built-in function max in module builtins:

max(...)
    max(iterable, *[, default=obj, key=func]) -> value
    max(arg1, arg2, *args, *[, key=func]) -> value

    With a single iterable argument, return its biggest item. The
    default keyword-only argument specifies an object to return if
    the provided iterable is empty.
    With two or more ...arguments, return the largest argument.
```

## Using `?` in IPython

Because access to additional information is so useful, IPython uses the `?` character as a shorthand for accessing this documentation along with other relevant information. IPython is a command shell for interactive computing in multiple languages.

For example:

```python
max?
```
```
max(iterable, *[, default=obj, key=func]) -> value
max(arg1, arg2, *args, *[, key=func]) -> value

With a single iterable argument, return its biggest item. The
default keyword-only argument specifies an object to return if
the provided iterable is empty.
With two or more arguments, return the largest argument.
Type:      builtin_function_or_method
```

You can even use this notation for object methods and objects themselves.

Let's say you create this array:

```python
a = np.array([1, 2, 3, 4, 5, 6])
```

You can obtain a lot of useful information (first details about `a` itself, followed by the docstring of `ndarray`, of which `a` is an instance):

```python
a?
```
```
Type:            ndarray
String form:     [1 2 3 4 5 6]
Length:          6
File:            ~/anaconda3/lib/python3.9/site-packages/numpy/__init__.py
Docstring:       <no docstring>
Class docstring:
ndarray(shape, dtype=float, buffer=None, offset=0,
        strides=None, order=None)

An array object represents a multidimensional, homogeneous array
of fixed-size items. An associated data-type object describes the
format of each element in the array (its byte-order, how many bytes it
occupies in memory, whether it is an integer, a floating point number,
or something else, etc.)
...
```

## Adding Docstrings to Your Own Functions

This also works for functions and other objects that you create. Just remember to include a docstring with your function using a string literal (`""" """` or `''' '''`) around your documentation.

For example, if you create this function:

```python
def double(a):
    '''Return a * 2'''
    return a * 2
```

You can obtain information about the function:

```python
double?
```
```
Signature: double(a)
Docstring: Return a * 2
File:      ~/Desktop/<ipython-input-23-b5adf20be596>
Type:      function
```

## Using `??` to View Source Code

You can reach another level of information by reading the source code of the object you're interested in. Using a double question mark (`??`) allows you to access the source code.

For example:

```python
double??
```
```
Signature: double(a)
Source:
def double(a):
    '''Return a * 2'''
    return a * 2
File:      ~/Desktop/<ipython-input-23-b5adf20be596>
Type:      function
```

> **Note:** If the object in question is compiled in a language other than Python, using `??` will return the same information as `?`. This is common with built-in objects and types, for example `len?` and `len??` produce identical output, since `len` is implemented in C rather than Python.

---

## 📝 Study Notes

- **`?` and `??` are IPython/Jupyter-only** — they won't work in a plain `.py` script run from the terminal. In a regular script, stick to `help()` or `print(obj.__doc__)`.
- **`help()` works on the *class*, not just the instance**, when you call it on an object — that's why `a?` on an array `a` shows you documentation for the whole `ndarray` class, not just facts about that one array.
- **This is an underused study habit:** whenever you're unsure what a NumPy function does or what arguments it accepts, `help(np.function_name)` is usually faster than a web search and always matches your installed version exactly.

**Try it yourself:**
1. Run `help(np.reshape)` and identify all its optional parameters.
2. Write a function with a docstring, then use `help()` on it to confirm your docstring displays correctly.
