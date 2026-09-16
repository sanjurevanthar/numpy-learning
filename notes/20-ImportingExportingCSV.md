# Importing and Exporting a CSV

It's simple to read in a CSV that contains existing information. The best and easiest way to do this is to use **Pandas**.

## Importing a CSV

```python
import pandas as pd

# If all of your columns are the same type:
x = pd.read_csv('music.csv', header=0).values
print(x)
# [['Billie Holiday' 'Jazz' 1300000 27000000]
#  ['Jimmie Hendrix' 'Rock' 2700000 70000000]
#  ['Miles Davis' 'Jazz' 1500000 48000000]
#  ['SIA' 'Pop' 2000000 74000000]]

# You can also simply select the columns you need:
x = pd.read_csv('music.csv', usecols=['Artist', 'Plays']).values
print(x)
# [['Billie Holiday' 27000000]
#  ['Jimmie Hendrix' 70000000]
#  ['Miles Davis' 48000000]
#  ['SIA' 74000000]]
```

## Exporting to a CSV

It's simple to use Pandas to export your array as well. If you are new to NumPy, you may want to create a Pandas dataframe from the values in your array and then write the dataframe to a CSV file with Pandas.

If you created this array `a`:

```python
import numpy as np

a = np.array([[-2.58289208,  0.43014843, -1.24082018, 1.59572603],
              [ 0.99027828, 1.17150989,  0.94125714, -0.14692469],
              [ 0.76989341,  0.81299683, -0.95068423, 0.11769564],
              [ 0.20484034,  0.34784527,  1.96979195, 0.51992837]])
```

You could create a Pandas dataframe:

```python
df = pd.DataFrame(a)
print(df)
#           0         1         2         3
# 0 -2.582892  0.430148 -1.240820  1.595726
# 1  0.990278  1.171510  0.941257 -0.146925
# 2  0.769893  0.812997 -0.950684  0.117696
# 3  0.204840  0.347845  1.969792  0.519928
```

You can easily save your dataframe with:

```python
df.to_csv('pd.csv')
```

And read your CSV with:

```python
data = pd.read_csv('pd.csv')
```

## Exporting Directly with NumPy

You can also save your array with the NumPy `savetxt` method:

```python
np.savetxt('np.csv', a, fmt='%.2f', delimiter=',', header='1,  2,  3,  4')
```

If you're using the command line, you can read your saved CSV any time with a command such as:

```bash
$ cat np.csv
#  1,  2,  3,  4
-2.58,0.43,-1.24,1.60
0.99,1.17,0.94,-0.15
0.77,0.81,-0.95,0.12
0.20,0.35,1.97,0.52
```

Or you can open the file any time with a text editor.

If you're interested in learning more about Pandas, take a look at the [official Pandas documentation](https://pandas.pydata.org/docs/) and the [Pandas installation guide](https://pandas.pydata.org/docs/getting_started/install.html).

## See Also

- [How to Save and Load NumPy Objects](19-SaveLoadArrays.md) — NumPy-native binary and text formats.

---

## 📝 Study Notes

- **`.values` converts a Pandas DataFrame to a NumPy array**, but you lose column names and per-column dtypes in the process — if your CSV mixes strings and numbers, the resulting array is often forced to `dtype=object`, which is much slower for numeric work. Consider keeping numeric and non-numeric columns separate.
- **Pandas vs. `np.genfromtxt`/`np.loadtxt`:** Pandas is far more forgiving of messy real-world CSVs (missing values, mixed types, headers) — reach for plain NumPy text I/O only when your data is already clean and purely numeric.
- **`index=False` is worth knowing** when exporting: `df.to_csv('file.csv')` writes an extra unnamed index column by default; `df.to_csv('file.csv', index=False)` skips it, which is usually what you want when sharing a CSV.

**Try it yourself:**
1. Load a CSV with `pd.read_csv()`, then check `.dtypes` per column before converting to a NumPy array — did any numeric-looking column come back as `object`?
2. Export a DataFrame both with and without `index=False` and compare the two files.
