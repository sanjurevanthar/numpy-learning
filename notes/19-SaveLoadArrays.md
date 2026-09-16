# How to Save and Load NumPy Objects

This section covers `np.save`, `np.savez`, `np.savetxt`, `np.load`, and `np.loadtxt`.

You will, at some point, want to save your arrays to disk and load them back without having to re-run the code. Fortunately, there are several ways to save and load objects with NumPy.

The `ndarray` objects can be saved to and loaded from disk files with:
- `loadtxt` and `savetxt` functions that handle normal text files,
- `load` and `save` functions that handle NumPy binary files with a `.npy` file extension, and
- a `savez` function that handles NumPy files with a `.npz` file extension.

The `.npy` and `.npz` files store data, shape, dtype, and other information required to reconstruct the `ndarray` in a way that allows the array to be correctly retrieved, even when the file is on another machine with a different architecture.

- If you want to store a **single** `ndarray` object, store it as a `.npy` file using `np.save`.
- If you want to store **more than one** `ndarray` object in a single file, save it as a `.npz` file using `np.savez`.
- You can also save several arrays into a single file in **compressed** npz format with `savez_compressed`.

## Saving and Loading a Binary Array (`.npy`)

It's easy to save and load an array with `np.save()`. Just make sure to specify the array you want to save and a file name.

For example, if you create this array:

```python
import numpy as np

a = np.array([1, 2, 3, 4, 5, 6])
```

You can save it as `filename.npy` with:

```python
np.save('filename', a)
```

You can use `np.load()` to reconstruct your array:

```python
b = np.load('filename.npy')

print(b)
# [1 2 3 4 5 6]
```

## Saving and Loading a Plain Text File

You can save a NumPy array as a plain text file, like a `.csv` or `.txt` file, with `np.savetxt`.

For example, if you create this array:

```python
csv_arr = np.array([1, 2, 3, 4, 5, 6, 7, 8])
```

You can easily save it as a `.csv` file with the name `new_file.csv` like this:

```python
np.savetxt('new_file.csv', csv_arr)
```

You can quickly and easily load your saved text file using `loadtxt()`:

```python
np.loadtxt('new_file.csv')
# array([1., 2., 3., 4., 5., 6., 7., 8.])
```

The `savetxt()` and `loadtxt()` functions accept additional optional parameters such as `header`, `footer`, and `delimiter`.

```python
np.savetxt('new_file.csv', csv_arr, delimiter=',', header='values', comments='')
```

## Choosing Between Binary and Text Formats

| | `.npy` / `.npz` | Text (`.csv` / `.txt`) |
|---|---|---|
| File size | Smaller | Larger |
| Read/write speed | Faster | Slower |
| Human readable | No | Yes |
| Cross-tool sharing | NumPy/Python only | Easy to share with other tools |
| Preserves dtype/shape exactly | Yes | Not always (needs re-parsing) |

While text files can be easier for sharing, `.npy` and `.npz` files are smaller and faster to read. If you need more sophisticated handling of your text file — for example, if you need to work with lines that contain missing values — you'll want to use the `genfromtxt` function.

Learn more about input and output routines in the [NumPy documentation](https://numpy.org/doc/stable/reference/routines.io.html).

## See Also

- [Importing and Exporting a CSV](20-ImportingExportingCSV.md) — using Pandas for CSV I/O.

---

## 📝 Study Notes

- **Always include the file extension awareness:** `np.save('filename', a)` automatically appends `.npy` for you even if you don't type it — but `np.load()` requires you to include the `.npy` extension explicitly. This asymmetry trips people up.
- **`.npz` files are essentially zip archives of multiple `.npy` arrays.** After `np.load('file.npz')`, you get back a dict-like object — access individual arrays by the keyword names you used when saving with `np.savez(file, arr1=a, arr2=b)`.
- **For anything you'll reload with NumPy itself, prefer `.npy`/`.npz` over `.csv`.** Text formats round-trip through string parsing, which can quietly lose float precision unless you're careful with format strings (`fmt='%.18e'` for full precision).

**Try it yourself:**
1. Save two different arrays into a single `.npz` file with descriptive keyword names, then reload and confirm both come back correctly.
2. Save a float array to `.csv` with only 2 decimal places of precision, reload it, and check whether the values still match exactly.
