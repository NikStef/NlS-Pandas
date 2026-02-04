# Netflix Data Cleaning Project

## Overview
A comprehensive data cleaning and preparation project using the Netflix titles dataset. This project demonstrates practical ETL processes, data quality assurance, and transformation techniques commonly used in data engineering workflows.

## Dataset
- **Source:** Netflix Titles Dataset
- **Size:** 8,000+ records
- **Format:** CSV
- **Key Fields:** show_id, type, title, director, cast, country, date_added, release_year, rating, duration, listed_in, description

## Data Cleaning Pipeline

### 1. Initial Data Exploration
```python
# Load and inspect dataset
df = pd.read_csv("netflix_titles.csv")
df.head()
df.shape  # Check dimensions
df.dtypes  # Verify data types
df.info()  # Get overview of data structure
```

### 2. Duplicate Detection and Removal
```python
# Remove duplicate entries
df = df.drop_duplicates()
```
**Result:** Ensured data integrity by eliminating redundant records

### 3. Missing Value Analysis
```python
# Comprehensive missing value assessment
df.isnull().sum().sort_values(ascending=False)

# Calculate percentage of missing values per column
for column in df.columns:
    percentage = df[column].isnull().mean()
    print(f"{column}: {percentage:.2%} missing values")
```

**Key Findings:**
- `director`: ~30% missing values
- `cast`: ~10% missing values  
- `country`: ~8% missing values
- `rating`: ~0.5% missing values
- `duration`: minimal missing values

### 4. Missing Value Treatment Strategies

#### Strategy 1: Dropping Rows (for critical fields)
```python
# Remove rows where director information is missing
df.dropna(subset=['director'])
```

#### Strategy 2: Mode Imputation (for categorical data)
```python
# Fill missing 'rating' values with the most common rating
mode_value = df['rating'].mode().iloc[0]
df['rating'] = df['rating'].fillna(mode_value)
```
**Rationale:** Mode is appropriate for categorical data like rating categories

#### Strategy 3: Placeholder Values
```python
# Fill missing 'duration' values with '0' for later processing
df['duration'] = df['duration'].fillna('0')
```

#### Alternative Strategies Explored:
```python
# Forward fill - copy value from previous row
df.ffill()

# Backward fill - copy value from next row  
df.bfill()
```

### 5. Feature Engineering & Data Transformation

#### Extracting Duration in Minutes (for Movies)
```python
# Filter to movies only
df_movies = df[df['type'] == 'Movie']

# Extract numeric minutes from duration string
# Example: "90 min" → 90
df_movies['minute'] = df_movies['duration'].str.split(expand=True)[0].astype(int)
```

**Technique Used:** String manipulation with `.str.split()` and type conversion

#### Extracting Year from Date
```python
# Method 1: String split on comma
df_movies['year_added'] = df_movies['date_added'].str.split(',', expand=True)[1]

# Method 2: Regex pattern extraction (more robust)
df_movies['year_added'] = df_movies['date_added'].str.extract('(\d{4})')
```

**Technique Used:** Regular expressions for pattern matching

### 6. Data Quality Validation
```python
# Verify no missing values remain in critical columns
df.isnull().sum()

# Check for any NaN values created during transformations
df_movies['duration'].str.split().str[0].isna().sum()
```

## Technical Skills Demonstrated

### pandas Operations
- ✅ **Data Loading:** `pd.read_csv()`
- ✅ **Data Exploration:** `.head()`, `.info()`, `.shape`, `.dtypes`
- ✅ **Duplicate Handling:** `.drop_duplicates()`
- ✅ **Missing Value Detection:** `.isnull()`, `.isna()`, `.sum()`
- ✅ **Missing Value Treatment:** `.dropna()`, `.fillna()`, `.ffill()`, `.bfill()`
- ✅ **String Manipulation:** `.str.split()`, `.str.extract()`
- ✅ **Type Conversion:** `.astype(int)`, `.astype('Int64')`
- ✅ **Filtering:** Boolean indexing (`df[df['type'] == 'Movie']`)
- ✅ **Statistical Functions:** `.mode()`, `.mean()`
- ✅ **Regular Expressions:** Pattern matching with `str.extract('(\d{4})')`

### Data Cleaning Best Practices
- ✅ Systematic approach to missing value handling
- ✅ Multiple strategies evaluated (drop, fill, placeholder)
- ✅ Appropriate imputation methods for data types
- ✅ Data type validation and conversion
- ✅ Feature extraction from string fields
- ✅ Data quality checks throughout process

### Problem-Solving Approach
1. **Analyze** missing value patterns
2. **Evaluate** multiple handling strategies
3. **Choose** appropriate method per column
4. **Transform** data for analysis readiness
5. **Validate** results at each step

## Key Insights

### Missing Value Decision Framework
| Column | % Missing | Strategy | Rationale |
|--------|-----------|----------|-----------|
| director | ~30% | Drop rows | Critical metadata |
| rating | ~0.5% | Mode fill | Categorical, low missing % |
| duration | <1% | Placeholder '0' | Needs transformation anyway |
| cast/country | 8-10% | Keep as-is | Non-critical for analysis |

### Data Type Conversions
- `duration` string → numeric minutes (int)
- `date_added` string → extracted year (int)
- Handled NaN values during conversion using `'Int64'` for nullable integers

## Technologies Used
- **Python 3.x**
- **pandas:** Core data manipulation
- **NumPy:** Supporting numerical operations
- **Jupyter Notebook:** Interactive development

## Project Structure
```
NlS-Pandas/
├── netflix_titles.csv          # Original dataset (8,000+ records)
├── netflix_cleaning.ipynb      # Complete cleaning pipeline
├── .gitignore                  # Git configuration
└── README.md                   # Project documentation
```

## How to Run

### Prerequisites
```bash
pip install pandas numpy jupyter
```

### Execution
```bash
# Clone repository
git clone https://github.com/NikStef/NlS-Pandas.git
cd NlS-Pandas

# Launch Jupyter Notebook
jupyter notebook netflix_cleaning.ipynb
```

## Results & Outputs
The cleaned dataset includes:
- ✅ No duplicate records
- ✅ Missing values handled appropriately
- ✅ New engineered features:
  - `minute` - Numeric duration for movies
  - `year_added` - Extracted year from date
- ✅ Standardized data types
- ✅ Ready for downstream analysis


## Author
Stefanos Nikolopoulos


---

*This project demonstrates practical data engineering skills including ETL pipeline development, data quality assurance, and preparation of analytics-ready datasets. Perfect for showcasing pandas proficiency in data engineering interviews.*
