# 🧹 Customer Data — EDA & Data Cleaning

A data cleaning and exploratory data analysis (EDA) project using a messy customer sales dataset. This project covers handling missing values, removing duplicates, fixing data types, and removing outliers using Python and Pandas.

---

## 📁 Dataset

- **File:** `messy_customer_sales_data.csv`
- **Rows:** 10,200
- **Columns:** 12
- **Columns include:** Customer_ID, Name, Gender, Age, City, Signup_Date, Last_Purchase_Date, Purchase_Amount, Feedback_Score, Email, Phone_Number, Country

---

## 🛠️ Tools & Libraries Used

| Tool | Purpose |
|------|---------|
| Python | Programming language |
| Pandas | Data manipulation and cleaning |
| NumPy | Numerical operations |
| Matplotlib | Data visualization |
| Seaborn | Statistical visualizations |
| Jupyter Notebook | Development environment |

---

## 🔍 Step 1 — Initial Data Exploration

Before cleaning, we explored the dataset to understand its structure and identify issues.

```python
df.shape        # (10200, 12) — 10200 rows, 12 columns
df.info()       # column types and non-null counts
df.describe()   # statistical summary
df.isnull().sum()  # count missing values per column
df.duplicated().sum()  # count duplicate rows — found 15
df.nunique()    # unique values per column
```

**Issues found:**
- 15 duplicate rows
- Missing values in Customer_ID (1023), Gender (1026), Age (951), City (1016), Country (732), Purchase_Amount (1021), Feedback_Score (1023)
- Age column stored as `object` (text) instead of numeric
- Age had impossible values — negative ages and ages above 100

---

## 🧹 Step 2 — Removing Duplicates

Found 15 exact duplicate rows in the dataset. Removed them using:

```python
df = df.drop_duplicates().reset_index(drop=True)
print(df.shape)  # (10185, 12) — 15 rows removed
```

**Before:** 10,200 rows  
**After:** 10,185 rows

---

## 🔧 Step 3 — Fixing Data Types

The `Age` column was stored as `object` (text) so statistical functions like mean, median, min, max were not working on it.

```python
# Step 1 — convert text to numeric
df['Age'] = pd.to_numeric(df['Age'], errors='coerce')
# errors='coerce' converts invalid values to NaN instead of crashing

# Step 2 — fill missing values with median
df['Age'] = df['Age'].fillna(df['Age'].median())

# Step 3 — convert float to integer
df['Age'] = df['Age'].astype(int)
```

**Before:** `Age` dtype = object  
**After:** `Age` dtype = int64

---

## 🚫 Step 4 — Removing Outliers in Age

After fixing the data type, `describe()` showed impossible age values:
- `min = -10` — impossible, age can't be negative
- `max = 250` — impossible, no human lives 250 years

Removed these rows by keeping only realistic ages (1 to 100):

```python
df = df[(df['Age'] >= 1) & (df['Age'] <= 100)].reset_index(drop=True)
```

**Rows removed:** Rows with Age < 1 or Age > 100

---

## 🩹 Step 5 — Handling Missing Values

Different strategies used depending on column type:

```python
# Numeric columns — fill with median or mean
df['Purchase_Amount'] = df['Purchase_Amount'].fillna(df['Purchase_Amount'].median())
df['Feedback_Score'] = df['Feedback_Score'].fillna(df['Feedback_Score'].mean())

# Text columns — fill with 'Unknown'
df['Gender'] = df['Gender'].fillna('Unknown')
df['City'] = df['City'].fillna('Unknown')
df['Country'] = df['Country'].fillna('Unknown')
df['Customer_ID'] = df['Customer_ID'].fillna('Unknown')
df['Last_Purchase_Date'] = df['Last_Purchase_Date'].fillna('Unknown')
```

**Why median for numeric?** Median is not affected by extreme values (outliers).  
**Why mean for Feedback_Score?** Score is between 1–10, no extreme outliers expected.  
**Why 'Unknown' for text?** Can't use a number for text columns.

---

## ✅ Final Dataset Summary

```python
print(df.isnull().sum())   # all zeros — no missing values
print(df.shape)             # final row and column count
print(df['Age'].describe()) # min=1, max=100, realistic values
```

| Check | Before Cleaning | After Cleaning |
|-------|----------------|----------------|
| Total rows | 10,200 | ~10,100+ |
| Duplicate rows | 15 | 0 |
| Missing values | 7,000+ | 0 |
| Age data type | object | int64 |
| Impossible ages | Yes | No |

---

## 📂 Project Structure

```
customer-data-eda-data-cleaning/
│
├── messy_customer_sales_data.csv    # original raw dataset
├── customer data-eda-data cleaning.ipynb  # main notebook
└── README.md                        # project documentation
```

---

## 🚀 How to Run

1. Clone the repository
```bash
git clone https://github.com/Vinod-dyade/customer-data-eda-data-cleaning.git
```

2. Open Jupyter Notebook
```bash
jupyter notebook
```

3. Open `customer data-eda-data cleaning.ipynb` and run all cells top to bottom

---

## 👨‍💻 Author

**Vinod Dyade**  
📧 vinoddyade@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/vinod-dyade-367281301)  
🐙 [GitHub](https://github.com/Vinod-dyade)

---

*This is part of my Data Science learning journey. More projects coming soon!*
