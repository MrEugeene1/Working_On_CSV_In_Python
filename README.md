# 📍 Reading CSV Files with Pandas

A hands-on Jupyter notebook that teaches you how to load CSV files with **pandas**, and how to fix the problems that come up most often with real CSV files: comment lines at the top, data split across folders, non-comma delimiters, very large files and very wide tables.

All examples use the **BigMart Sales** dataset: 8,523 sales records for 1,559 products across 10 stores, with 12 columns.

---

## 📚 What You'll Learn

| # | Topic | Key pandas / Python feature |
|---|-------|-----------------------------|
| 1 | Read a CSV into a DataFrame and inspect it | `pd.read_csv()`, `.head()`, `.shape` |
| 2 | Skip comment or junk lines at the top of a file | `skiprows=n` |
| 3 | Read and combine CSVs spread across many folders | `glob.glob()`, `pd.concat()` |
| 4 | Save a DataFrame back to CSV | `.to_csv(index=False)` |
| 5 | Read files that use a different delimiter (e.g. tab) | `delimiter='\t'` / `sep='\t'` |
| 6 | Load only the first *N* rows of a large file | `nrows=n` |
| 7 | Load only the columns you need | `usecols=[...]` |

---

## 🗂️ Project Structure

```
reading_csv/
├── 1_CSV_Files.ipynb                     # The main tutorial notebook
├── README.md
└── datasets/
    ├── big_mart_sales.csv                # Clean dataset (8,523 rows × 12 cols)
    ├── big_mart_sales_top_row_error.csv  # Same data with 5 comment lines on top
    ├── big_mart_sales_delimiter.csv      # Tab-separated sample
    ├── big_mart_sales_encoded.csv        # Copy of the clean dataset
    ├── i.csv                             # Sample subset that keeps its original index column
    ├── merged_big_mart_sales.csv         # Created by the notebook (combined yearly files)
    └── multi-directory/                  # Data split by outlet establishment year
        ├── 1985/1985.csv
        ├── 1987/1987.csv
        ├── 1997/1997.csv
        ├── 1998/1998.csv
        ├── 1999/1999.csv
        ├── 2002/2002.csv
        ├── 2004/2004.csv
        ├── 2007/2007.csv
        └── 2009/2009.csv
```

---

## 🧾 About the Dataset

The BigMart Sales data was collected for 1,559 products across 10 stores in different cities.

| Column | Description |
|--------|-------------|
| `Item_Identifier` | Unique product ID |
| `Item_Weight` | Weight of the product |
| `Item_Fat_Content` | Low Fat / Regular |
| `Item_Visibility` | Share of the store's display area given to the product |
| `Item_Type` | Product category (Dairy, Meat, Soft Drinks, …) |
| `Item_MRP` | Maximum retail price |
| `Outlet_Identifier` | Unique store ID |
| `Outlet_Establishment_Year` | Year the store opened |
| `Outlet_Size` | Small / Medium / High |
| `Outlet_Location_Type` | Tier of the city the store is in |
| `Outlet_Type` | Grocery store or supermarket type |
| `Item_Outlet_Sales` | Product sales in that store (target variable) |

> ⚠️ Some columns, such as `Item_Weight` and `Outlet_Size`, contain missing values.

---

## ⚙️ Getting Started

### Prerequisites
- Python 3.9+
- Jupyter Notebook, JupyterLab, or VS Code with the Jupyter extension

### Installation

```bash
# (optional) create and activate a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # macOS / Linux

# install dependencies
pip install pandas jupyter
```

### Run the notebook

```bash
jupyter notebook 1_CSV_Files.ipynb
```

> 💡 Run the notebook from the `reading_csv/` folder. The code uses relative paths such as `datasets/big_mart_sales.csv`, so it won't find the files if you start it from somewhere else.

---

## 🧪 Quick Reference (Cheat Sheet)

```python
import pandas as pd
import glob

# 1. Basic read
data = pd.read_csv('datasets/big_mart_sales.csv')
data.head()        # first 5 rows
data.shape         # (8523, 12)

# 2. Skip comment lines at the top of the file
df = pd.read_csv('datasets/big_mart_sales_top_row_error.csv', skiprows=5)

# 3. Read & merge CSVs from multiple directories
frames = [pd.read_csv(f) for f in glob.glob('datasets/multi-directory/*/*.csv')]
merged = pd.concat(frames, ignore_index=True)

# 4. Save to CSV (without writing the index)
merged.to_csv('datasets/merged_big_mart_sales.csv', index=False)

# 5. Custom delimiter (tab-separated file)
df = pd.read_csv('datasets/big_mart_sales_delimiter.csv', delimiter='\t')

# 6. Read only the first N rows
sample = pd.read_csv('datasets/big_mart_sales.csv', nrows=100)

# 7. Read only specific columns
cols = pd.read_csv('datasets/big_mart_sales.csv',
                   usecols=['Item_Identifier', 'Item_Type', 'Item_MRP', 'Item_Outlet_Sales'])
```

---

## 🛠️ Common Errors & Fixes

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `ParserError: Error tokenizing data` | Comments or notes above the header row | Open the file in a text editor, count the extra lines, then pass `skiprows=n` |
| Everything ends up in **one column**, with `\t` visible in the values | The file isn't comma-separated | Pass `sep='\t'` (or `';'`, `'|'`, …) |
| An extra `Unnamed: 0` column appears | The file was saved with its index | Read with `index_col=0`, or save with `index=False` |
| `FileNotFoundError` | Wrong working directory | Run from `reading_csv/` or use an absolute path |
| `UnicodeDecodeError` | The file isn't UTF-8 | Pass `encoding='latin-1'` (or `'cp1252'`, `'utf-16'`) |
| Memory errors or very slow loading | The file is very large | Use `nrows`, `usecols`, or `chunksize` |

---

## 🚀 Next Steps

This notebook is the first in a series on reading data. Natural next topics:
- Reading **Excel** files (`pd.read_excel`)
- Reading **JSON** files (`pd.read_json`)
- Reading **text** files and other formats

---

## 🔗 Resources

- [pandas documentation](https://pandas.pydata.org/docs/)
- [`pandas.read_csv` API reference](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)
- [Python `glob` module](https://docs.python.org/3/library/glob.html)

---

## 📄 License

This project is for educational purposes. The BigMart Sales dataset is publicly available for learning and practice.
