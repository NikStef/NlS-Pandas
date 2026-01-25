# Netflix Data Cleaning & Exploration (Pandas)

This project focuses on cleaning and exploring the **Netflix Titles dataset** using **Python and pandas**. The goal is to practice real-world data cleaning techniques, understand common data issues, and apply appropriate solutions.

---

## 📁 Dataset

* **Source:** Netflix Titles dataset (CSV)
* **Rows:** ~6,000+
* **Columns:** 12
* **Content types:** Movies and TV Shows

---

## 🧹 Data Cleaning Steps

### 1. Initial Inspection

* Loaded the dataset using `pandas.read_csv()`
* Checked structure with:

  * `head()`
  * `info()`
  * `shape`
  * `dtypes`

---

### 2. Duplicate Removal

* Removed duplicate rows using:

  ```python
  df = df.drop_duplicates()
  ```

---

### 3. Missing Values Analysis

* Identified missing values per column:

  ```python
  df.isnull().sum()
  ```
* Calculated percentage of missing values per column

---

### 4. Handling Categorical Missing Data

#### Ratings

* Filled missing `rating` values using the **mode** (most frequent value)
* Used pandas nullable integer/string-safe methods to avoid chained assignment issues

#### Director / Cast / Country

* Analyzed missingness
* Decided whether to keep, drop, or leave missing values depending on analytical relevance

---

### 5. Duration Cleaning (Movies)

* Filtered only movie records:

  ```python
  df_movies = df[df['type'] == 'Movie']
  ```

* Extracted numeric duration (minutes) from strings like `"90 min"`

* Used regex to safely extract numbers:

  ```python
  df_movies['duration_minutes'] = (
      df_movies['duration']
      .str.extract(r'(\d+)')
      .astype('Int64')
  )
  ```

* Learned why `Int64` works better than `int` when missing values are present

---

### 6. Date Cleaning (`date_added`)

* Split and extracted the **year** from the `date_added` column:

  ```python
  df_movies['year_added'] = df_movies['date_added'].str.extract('(\d{4})')
  ```

* Demonstrated both `split()` and `extract()` approaches

* Preferred regex for robustness

---

## 🧠 Key Concepts Learned

* Difference between `int` and pandas nullable `Int64`
* How transformations can introduce missing values
* Why `inplace=True` is discouraged in modern pandas
* When to use:

  * **mean / median** → numeric data
  * **mode** → categorical data
  * **ffill / bfill** → ordered or time-series data
* Importance of assigning results back to the DataFrame

---

## 🛠 Tools Used

* Python 3
* pandas
* NumPy
* VS Code / Jupyter Notebook
* Git & GitHub

---

## 📌 Conclusion

This project demonstrates practical data cleaning workflows using pandas, highlights common pitfalls in real datasets, and reinforces best practices for handling missing, categorical, and messy string-based data.

It serves as a foundational exercise in preparing data for analysis or modeling.
