# 📊 Data Science — Cheatsheet

> A comprehensive reference covering the Data Science lifecycle, tools, statistics, EDA, visualization, and best practices.

---

## Table of Contents

1. [What is Data Science?](#what-is-data-science)
2. [Data Science Lifecycle](#data-science-lifecycle)
3. [Statistics Fundamentals](#statistics-fundamentals)
4. [Probability Basics](#probability-basics)
5. [NumPy Essentials](#numpy-essentials)
6. [Pandas Essentials](#pandas-essentials)
7. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
8. [Data Visualization](#data-visualization)
9. [Data Cleaning](#data-cleaning)
10. [Feature Engineering](#feature-engineering)
11. [SQL for Data Science](#sql-for-data-science)
12. [Common Data Formats](#common-data-formats)
13. [Tools & Ecosystem](#tools--ecosystem)

---

## What is Data Science?

Data Science is an interdisciplinary field that uses **statistics, computing, and domain expertise** to extract actionable insights from structured and unstructured data.

```
┌──────────────────────────────────────────┐
│             DATA SCIENCE                 │
│                                          │
│    Math/Stats ∩ CS/Programming           │
│              ∩                           │
│       Domain Knowledge                   │
│                                          │
│  Statistics · ML · Visualization         │
│  Data Engineering · Communication        │
└──────────────────────────────────────────┘
```

### Data Science vs Related Fields

| Field                | Focus                                        |
|----------------------|----------------------------------------------|
| **Data Science**     | End-to-end insight extraction & prediction   |
| **Data Analytics**   | Descriptive analysis, dashboards, BI         |
| **Data Engineering** | Pipelines, infrastructure, ETL               |
| **Machine Learning** | Predictive model building                    |
| **Statistics**       | Inference, hypothesis testing                |

---

## Data Science Lifecycle

```
1. Problem Definition     → What question are we answering?
2. Data Collection        → APIs, databases, web scraping, files
3. Data Cleaning          → Handle missing/inconsistent data
4. Exploratory Analysis   → Understand distributions & relationships
5. Feature Engineering    → Create meaningful variables
6. Modeling               → Train ML models
7. Evaluation             → Validate performance
8. Communication          → Visualize & present findings
9. Deployment             → Put model into production
10. Monitoring            → Track performance over time
```

---

## Statistics Fundamentals

### Descriptive Statistics

```python
import numpy as np
import pandas as pd

data = [12, 15, 18, 22, 25, 28, 30, 35, 40, 100]

np.mean(data)       # Mean (average): 32.5
np.median(data)     # Median (middle value): 26.5
# Mode: most frequent (use scipy.stats.mode)
np.std(data)        # Standard deviation
np.var(data)        # Variance
np.percentile(data, [25, 50, 75])  # Quartiles

# Pandas describe
df['col'].describe()   # count, mean, std, min, 25%, 50%, 75%, max
```

### Measures of Central Tendency

| Measure    | Best For                           | Sensitive to Outliers? |
|------------|------------------------------------|------------------------|
| **Mean**   | Symmetric distributions            | ✅ Yes                 |
| **Median** | Skewed data, outliers present      | ❌ No                  |
| **Mode**   | Categorical data                   | ❌ No                  |

### Measures of Spread

| Measure               | Formula / Description                        |
|-----------------------|----------------------------------------------|
| **Range**             | max - min                                    |
| **Variance (σ²)**     | Average squared deviation from mean          |
| **Std Deviation (σ)** | √Variance — same units as data               |
| **IQR**               | Q3 - Q1 (middle 50%)                         |
| **Coefficient of Variation** | (σ / μ) × 100%                        |

### Distributions

| Distribution     | Type       | Use Case                            |
|------------------|------------|-------------------------------------|
| **Normal**       | Continuous | Natural phenomena, errors           |
| **Binomial**     | Discrete   | Success/failure in n trials         |
| **Poisson**      | Discrete   | Events per time interval            |
| **Uniform**      | Both       | Equal probability                   |
| **Exponential**  | Continuous | Time between events                 |
| **Log-Normal**   | Continuous | Income, stock prices                |

### The Normal Distribution (Gaussian)

```
68-95-99.7 Rule:
  68%  of data within ± 1σ of mean
  95%  of data within ± 2σ of mean
  99.7% of data within ± 3σ of mean
```

### Hypothesis Testing

```
1. State null hypothesis H₀ (no effect) and alternative H₁
2. Choose significance level α (typically 0.05)
3. Compute test statistic
4. Find p-value
5. If p-value < α → reject H₀
```

| Test               | Use Case                                  |
|--------------------|-------------------------------------------|
| **t-test**         | Compare means of two groups               |
| **Chi-squared**    | Test independence of categorical vars     |
| **ANOVA**          | Compare means of 3+ groups               |
| **Pearson r**      | Linear correlation between two variables  |
| **Mann-Whitney U** | Non-parametric comparison of two groups   |

```python
from scipy import stats

# t-test (two independent samples)
t_stat, p_val = stats.ttest_ind(group_a, group_b)

# Chi-squared test
chi2, p_val, dof, expected = stats.chi2_contingency(contingency_table)

# Pearson correlation
r, p_val = stats.pearsonr(x, y)
```

### Correlation vs Causation

> **Correlation ≠ Causation.** Two variables can be correlated without one causing the other. Always consider confounding variables, reverse causality, and coincidence.

| Correlation Value | Strength     |
|-------------------|--------------|
| 0.0 – 0.3        | Weak         |
| 0.3 – 0.7        | Moderate     |
| 0.7 – 1.0        | Strong       |

---

## Probability Basics

```
P(A)              = Probability of event A
P(A ∩ B)          = Probability of A AND B
P(A ∪ B)          = P(A) + P(B) - P(A ∩ B)
P(A|B)            = P(A ∩ B) / P(B)            — Conditional
P(A) × P(B)       = P(A ∩ B)                   — If independent

Bayes' Theorem:
P(A|B) = P(B|A) × P(A) / P(B)
```

---

## NumPy Essentials

```python
import numpy as np

# Array creation
arr = np.array([1, 2, 3, 4, 5])
zeros = np.zeros((3, 4))          # 3×4 matrix of zeros
ones = np.ones((2, 3))            # 2×3 matrix of ones
rng = np.arange(0, 10, 2)         # [0, 2, 4, 6, 8]
lin = np.linspace(0, 1, 5)        # 5 evenly spaced points
rand = np.random.randn(3, 3)     # 3×3 standard normal

# Properties
arr.shape     arr.dtype     arr.ndim     arr.size

# Indexing & slicing
arr[0]      arr[-1]     arr[1:4]    arr[::2]
matrix[0, :]            # First row
matrix[:, 0]            # First column

# Operations (element-wise)
arr + 10     arr * 2     arr ** 2     np.sqrt(arr)
np.dot(a, b)   a @ b    # Matrix multiplication

# Aggregations
arr.sum()    arr.mean()   arr.std()   arr.min()   arr.max()
arr.argmax()             # Index of max value
np.cumsum(arr)           # Cumulative sum

# Reshaping
arr.reshape(5, 1)        # Reshape
arr.flatten()            # To 1D
arr.T                    # Transpose

# Boolean indexing
arr[arr > 3]             # Elements > 3
np.where(arr > 3, arr, 0)  # Conditional replacement
```

---

## Pandas Essentials

### DataFrame Basics

```python
import pandas as pd

# Create
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'salary': [50000, 60000, 70000]
})

# Load data
df = pd.read_csv('data.csv')
df = pd.read_excel('data.xlsx')
df = pd.read_json('data.json')
df = pd.read_sql('SELECT * FROM table', connection)

# Save
df.to_csv('output.csv', index=False)

# Inspection
df.head()       df.tail()       df.shape
df.info()       df.describe()   df.dtypes
df.columns      df.index        df.nunique()
df.value_counts('col')
```

### Selection & Filtering

```python
df['name']                   # Single column (Series)
df[['name', 'age']]          # Multiple columns (DataFrame)
df.loc[0]                    # Row by label
df.iloc[0]                   # Row by position
df.loc[0:2, 'name':'age']   # Label-based slicing
df.iloc[0:3, 0:2]           # Position-based slicing

# Filtering
df[df['age'] > 25]
df[(df['age'] > 25) & (df['salary'] > 55000)]
df[df['name'].isin(['Alice', 'Bob'])]
df.query('age > 25 and salary > 55000')
```

### Data Manipulation

```python
# Add/modify columns
df['bonus'] = df['salary'] * 0.1
df['senior'] = df['age'].apply(lambda x: x > 30)

# Sorting
df.sort_values('age', ascending=False)
df.sort_values(['age', 'salary'], ascending=[True, False])

# Renaming
df.rename(columns={'name': 'full_name'}, inplace=True)

# Drop
df.drop(columns=['bonus'])
df.drop(index=[0, 1])

# GroupBy
df.groupby('department')['salary'].mean()
df.groupby('department').agg({'salary': ['mean', 'max'], 'age': 'count'})

# Pivot Table
pd.pivot_table(df, values='salary', index='department',
               columns='gender', aggfunc='mean')

# Merge / Join
pd.merge(df1, df2, on='id', how='left')    # left, right, inner, outer
pd.concat([df1, df2], axis=0)               # Stack vertically

# Apply functions
df['age_group'] = df['age'].apply(lambda x: 'young' if x < 30 else 'senior')
df['name_len'] = df['name'].str.len()
```

### Handling Missing Data

```python
df.isnull().sum()                      # Count nulls per column
df.dropna()                            # Drop rows with any NaN
df.dropna(subset=['age'])              # Drop where 'age' is NaN
df['age'].fillna(df['age'].median())   # Fill with median
df.interpolate()                       # Interpolate missing values
df.ffill()                             # Forward fill
df.bfill()                             # Backward fill
```

### String Operations

```python
df['name'].str.lower()
df['name'].str.upper()
df['name'].str.contains('Ali')
df['name'].str.replace('a', 'A')
df['name'].str.split(' ')
df['name'].str.strip()
df['name'].str.len()
```

### Date Operations

```python
df['date'] = pd.to_datetime(df['date'])
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day_of_week'] = df['date'].dt.day_name()
df.set_index('date').resample('M').mean()   # Monthly average
```

---

## Exploratory Data Analysis (EDA)

### EDA Checklist

```
☐  Shape: How many rows/columns?
☐  Data types: Are they correct?
☐  Missing values: How many? Pattern?
☐  Duplicates: Any duplicate rows?
☐  Distributions: Normal? Skewed? Outliers?
☐  Correlations: Which features are related?
☐  Target variable: Balanced classes? Range?
☐  Cardinality: Unique values per categorical feature?
```

### Quick EDA Code

```python
# Shape & types
print(f"Shape: {df.shape}")
print(df.dtypes)
print(df.describe(include='all'))

# Missing values
print(df.isnull().sum().sort_values(ascending=False))
print(f"Missing %:\n{df.isnull().mean() * 100}")

# Duplicates
print(f"Duplicates: {df.duplicated().sum()}")
df.drop_duplicates(inplace=True)

# Correlations
corr_matrix = df.select_dtypes(include='number').corr()

# Outliers (IQR method)
Q1, Q3 = df['col'].quantile([0.25, 0.75])
IQR = Q3 - Q1
outliers = df[(df['col'] < Q1 - 1.5*IQR) | (df['col'] > Q3 + 1.5*IQR)]
```

---

## Data Visualization

### Matplotlib

```python
import matplotlib.pyplot as plt

# Line plot
plt.plot(x, y, color='blue', linestyle='--', marker='o')
plt.xlabel('X'); plt.ylabel('Y'); plt.title('Title')
plt.legend(); plt.grid(True); plt.show()

# Subplots
fig, axes = plt.subplots(1, 2, figsize=(12, 5))
axes[0].plot(x, y)
axes[1].bar(categories, values)
plt.tight_layout(); plt.show()
```

### Seaborn

```python
import seaborn as sns

sns.histplot(df['age'], bins=20, kde=True)     # Histogram + KDE
sns.boxplot(x='dept', y='salary', data=df)     # Box plot
sns.scatterplot(x='age', y='salary', hue='gender', data=df)
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm')  # Correlation
sns.countplot(x='category', data=df)           # Bar count
sns.pairplot(df, hue='target')                 # Pair plot
sns.violinplot(x='dept', y='salary', data=df)  # Violin plot
```

### When to Use Which Chart

| Chart Type      | Use Case                                   |
|-----------------|--------------------------------------------|
| **Line**        | Trends over time                           |
| **Bar**         | Comparing categories                       |
| **Histogram**   | Distribution of continuous variable        |
| **Box Plot**    | Distribution + outliers                    |
| **Scatter**     | Relationship between two variables         |
| **Heatmap**     | Correlation matrix, 2D distributions       |
| **Pie**         | Proportions (use sparingly)                |
| **Violin**      | Distribution shape comparison              |
| **Pair Plot**   | All pairwise relationships                 |

---

## Data Cleaning

### Common Issues & Solutions

| Issue                  | Detection                         | Solution                       |
|------------------------|-----------------------------------|--------------------------------|
| Missing values         | `df.isnull().sum()`               | Impute, drop, flag             |
| Duplicates             | `df.duplicated().sum()`           | `df.drop_duplicates()`         |
| Wrong data types       | `df.dtypes`                       | `df.astype()`, `pd.to_numeric` |
| Outliers               | Box plots, IQR, Z-score          | Cap, remove, transform        |
| Inconsistent formats   | `df['col'].unique()`             | `str.strip().lower()`         |
| Incorrect values       | Domain knowledge                  | Replace, remove               |
| Mixed types in column  | `df['col'].apply(type).unique()` | Convert or split               |

### Outlier Detection

```python
# Z-score method
from scipy import stats
z_scores = np.abs(stats.zscore(df['col']))
outliers = df[z_scores > 3]

# IQR method
Q1, Q3 = df['col'].quantile([0.25, 0.75])
IQR = Q3 - Q1
mask = (df['col'] >= Q1 - 1.5*IQR) & (df['col'] <= Q3 + 1.5*IQR)
df_clean = df[mask]
```

---

## Feature Engineering

```python
# Binning
df['age_group'] = pd.cut(df['age'], bins=[0,18,35,50,100],
                          labels=['Youth','Adult','Middle','Senior'])

# Log transformation (reduce skewness)
df['log_salary'] = np.log1p(df['salary'])

# Polynomial features
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly.fit_transform(X)

# Date features
df['days_since'] = (pd.Timestamp.now() - df['date']).dt.days
df['is_weekend'] = df['date'].dt.dayofweek >= 5

# Text features
df['text_length'] = df['text'].str.len()
df['word_count'] = df['text'].str.split().str.len()
```

---

## SQL for Data Science

```sql
-- Basic queries
SELECT col1, col2 FROM table WHERE condition;
SELECT DISTINCT col FROM table;
SELECT col, COUNT(*) FROM table GROUP BY col HAVING COUNT(*) > 5;

-- Aggregations
SELECT AVG(salary), MAX(salary), MIN(salary), COUNT(*) FROM employees;

-- Joins
SELECT a.name, b.dept
FROM employees a
INNER JOIN departments b ON a.dept_id = b.id;

-- Subqueries
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Window functions
SELECT name, salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank
FROM employees;

-- CTEs
WITH dept_avg AS (
    SELECT dept, AVG(salary) as avg_sal FROM employees GROUP BY dept
)
SELECT e.name, d.avg_sal
FROM employees e JOIN dept_avg d ON e.dept = d.dept;
```

---

## Common Data Formats

| Format   | Extension | Pros                           | Use Case                  |
|----------|-----------|--------------------------------|---------------------------|
| **CSV**  | .csv      | Simple, universal              | Tabular data exchange     |
| **JSON** | .json     | Hierarchical, web-friendly     | APIs, config, NoSQL       |
| **Parquet** | .parquet | Fast, compressed, columnar  | Big data, analytics       |
| **Excel**| .xlsx     | Familiar, multi-sheet          | Business reports          |
| **SQL**  | .db/.sql  | Structured, queryable          | Relational databases      |
| **HDF5** | .h5       | Large datasets, fast I/O       | Scientific computing      |
| **Feather** | .feather | Fast pandas I/O             | Intermediate storage      |

---

## Tools & Ecosystem

### Python Libraries

| Category            | Libraries                                    |
|---------------------|----------------------------------------------|
| **Data Wrangling**  | Pandas, Polars, NumPy                        |
| **Visualization**   | Matplotlib, Seaborn, Plotly, Altair          |
| **ML**              | Scikit-learn, XGBoost, LightGBM              |
| **Deep Learning**   | TensorFlow, PyTorch, Keras                   |
| **NLP**             | NLTK, spaCy, Hugging Face Transformers       |
| **Big Data**        | PySpark, Dask, Vaex                          |
| **Web Scraping**    | BeautifulSoup, Scrapy, Selenium              |
| **Experiment Tracking** | MLflow, Weights & Biases, DVC            |

### Notebook Environments

| Tool              | Description                              |
|-------------------|------------------------------------------|
| **Jupyter**       | Interactive notebooks (local)            |
| **Google Colab**  | Cloud notebooks with free GPU            |
| **Kaggle**        | Competitions + notebooks + datasets      |
| **VS Code**       | IDE with notebook support                |

### BI & Dashboard Tools

| Tool          | Best For                              |
|---------------|---------------------------------------|
| **Tableau**   | Enterprise dashboards                 |
| **Power BI**  | Microsoft ecosystem                   |
| **Streamlit** | Quick Python web apps                 |
| **Gradio**    | ML model demos                        |

---

> **Last Updated:** April 2026
