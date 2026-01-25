# Netflix Data Cleaning & Exploration (Pandas)

This project documents a hands-on data cleaning and exploration workflow using the **Netflix Titles dataset** and **Python (pandas)**. The notebook focuses on understanding real-world data issues and applying appropriate cleaning techniques rather than aggressive data removal.

---

## 📁 Dataset

* **Source:** Netflix Titles CSV dataset
* **Size:** ~6,000+ rows, 12 columns
* **Content:** Movies and TV Shows

---

## 🔍 Initial Exploration

* Loaded the dataset with `pd.read_csv()`
* Inspected structure and schema using:

  * `head()`
  * `dtypes`
  * `shape`
  * `info()`

---

## 🧹 Data Cleaning Steps

### 1️⃣ Duplicate Handling

* Duplicates were removed using:

```python
df = df.drop_duplicates()
```


### 2️⃣ Missing Values Analysis

* Checked missing values per column:

```python
df.isnull().sum().sort_values(ascending=False)
```


### 3️⃣ Handling Missing Categorical Data

#### `director`

* Explored multiple approaches:

  * Dropping the column
  * Dropping rows where `director` is missing
  * Filtering rows using boolean logic (`~isnull()`)

---

### 4️⃣ Filling Missing Ratings (Categorical)

* Identified missing values in `rating`
* Filled missing values using the **mode** (most frequent value)

Two approaches were demonstrated:

```python
mode1 = ''.join(df['rating'].mode())
mode2 = df['rating'].mode().iloc[0]
```
```python
df['rating'] = df['rating'].fillna(mode1)
```

---

### 5️⃣ Understanding Mean vs Median vs Mode

* **Mean**: numeric data without strong outliers
* **Median**: numeric, skewed data with outliers
* **Mode**: categorical or discrete data (used for `rating`)

---

### 6️⃣ Duration Cleaning and Type Conversion

* Filled missing `duration` values with `'0'` **as a string** to allow string operations:

```python
df['duration'] = df['duration'].fillna('0')
```

```python
df_movies = df[df['type'] == 'Movie']
```

* Extracted movie duration in minutes:

```python
df_movies['minute'] = (
    df_movies['duration']
    .str.split(expand=True)[0]
    .astype(int)
)
```
---

### 7️⃣ Forward Fill & Backward Fill

* Discussed modern pandas behavior:
* Demonstrated backward fill for learning purposes

---

### 8️⃣ Date Cleaning (`date_added`)

* Extracted year information using two methods:

```python
df_movies['date_added'].str.split(',', expand=True)[1]
```

```python
df_movies['date_added'].str.extract('(\\d{4})')
```

* Regex extraction was preferred for robustness

---

---

## 🛠 Tools Used

* Python 3
* pandas
* NumPy
* Jupyter Notebook / VS Code
* Git & GitHub

---

## 📌 Conclusion

This notebook demonstrates a realistic data cleaning workflow using pandas, emphasizing understanding data behavior, debugging common issues, and applying best practices rather than blindly applying transformations. It serves as a solid foundation for further analysis or modeling.
