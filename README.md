# Data_cleaning_pandas
Here's a detailed example demonstrating key data cleaning activities in Pandas, using a sample dataset of customer records. I'll walk through various steps such as handling missing values, correcting data types, removing duplicates, and other essential data cleaning tasks.

### Sample Data
Let's assume you have the following dataset:

```python
import pandas as pd

# Sample customer data
data = {
    'Customer_ID': [1, 2, 3, 4, 5, 6, 6],
    'Name': ['Alice', 'Bob', 'Charlie', None, 'Eve', 'Frank', 'Frank'],
    'Age': [25, 30, None, 45, 22, 'Twenty-eight', 28],
    'Email': ['alice@example.com', 'bob@sample.com', None, 'david@ex.com', 'eve@example.com', 'frank@sample.com', 'frank@sample.com'],
    'Purchase_Amount': ['100.5', 200, 150, None, 50, 100, 100],
    'Join_Date': ['2021-01-15', '15-02-2021', '2021/03/20', '2021-04-22', None, '2021-05-11', '2021-05-11']
}

df = pd.DataFrame(data)
print(df)
```

### Initial DataFrame

| Customer_ID | Name    | Age          | Email             | Purchase_Amount | Join_Date   |
|-------------|---------|--------------|-------------------|-----------------|-------------|
| 1           | Alice   | 25           | alice@example.com  | 100.5           | 2021-01-15  |
| 2           | Bob     | 30           | bob@sample.com     | 200             | 15-02-2021  |
| 3           | Charlie | NaN          | None              | 150             | 2021/03/20  |
| 4           | None    | 45           | david@ex.com       | NaN             | 2021-04-22  |
| 5           | Eve     | 22           | eve@example.com    | 50              | None        |
| 6           | Frank   | Twenty-eight | frank@sample.com   | 100             | 2021-05-11  |
| 6           | Frank   | 28           | frank@sample.com   | 100             | 2021-05-11  |

### 1. Handling Missing Values
Missing values are common in real-world data. Let's handle them based on the context:

- **Drop rows where critical information (like Name or Email) is missing.**
- **Fill missing values for numerical fields with the mean or median.**

```python
# Dropping rows where Name or Email is missing
df = df.dropna(subset=['Name', 'Email'])

# Filling missing Age values with the median of the Age column
df['Age'] = pd.to_numeric(df['Age'], errors='coerce')  # Convert Age to numeric, set errors as NaN
df['Age'].fillna(df['Age'].median(), inplace=True)

# Filling missing Purchase_Amount values with the mean
df['Purchase_Amount'] = pd.to_numeric(df['Purchase_Amount'], errors='coerce')  # Convert to float
df['Purchase_Amount'].fillna(df['Purchase_Amount'].mean(), inplace=True)

print(df)
```

### 2. Correcting Data Types
Sometimes, data is stored in the wrong format. In this case, the `Age` and `Purchase_Amount` columns should be numeric, and `Join_Date` should be in datetime format.

```python
# Correcting Join_Date format
df['Join_Date'] = pd.to_datetime(df['Join_Date'], errors='coerce', dayfirst=True)

print(df.dtypes)
```

### 3. Handling Duplicates
Duplicates can cause errors in data analysis. Let's check for duplicate entries and remove them.

```python
# Removing duplicates based on Customer_ID and Email
df.drop_duplicates(subset=['Customer_ID', 'Email'], keep='first', inplace=True)

print(df)
```

### 4. Standardizing Data
Standardizing data ensures consistency. For example, let's convert the names to title case.

```python
# Standardizing Name column to title case
df['Name'] = df['Name'].str.title()

print(df)
```

### 5. Handling Outliers
You might want to remove or handle outliers for numerical columns, such as `Age` or `Purchase_Amount`.

```python
# Removing rows where Age is less than 18 (if you expect adult customers)
df = df[df['Age'] >= 18]

print(df)
```

### 6. Renaming Columns
Renaming columns can improve clarity or adhere to naming conventions.

```python
# Renaming columns for consistency
df.rename(columns={'Purchase_Amount': 'Amount_Spent'}, inplace=True)

print(df)
```

### Final Cleaned DataFrame

After cleaning, the DataFrame might look like this:

| Customer_ID | Name    | Age  | Email            | Amount_Spent | Join_Date   |
|-------------|---------|------|------------------|--------------|-------------|
| 1           | Alice   | 25   | alice@example.com | 100.5        | 2021-01-15  |
| 2           | Bob     | 30   | bob@sample.com    | 200.0        | 2021-02-15  |
| 5           | Eve     | 22   | eve@example.com   | 50.0         | NaT         |
| 6           | Frank   | 28   | frank@sample.com  | 100.0        | 2021-05-11  |

### Summary of Cleaning Steps:
1. **Handling missing values**: Dropped rows with missing critical information, and filled missing values with mean/median.
2. **Correcting data types**: Converted columns to their appropriate types (e.g., `Age` and `Purchase_Amount` to numeric).
3. **Removing duplicates**: Removed duplicate customer records based on key fields like `Customer_ID` and `Email`.
4. **Standardizing data**: Standardized text data (e.g., converting names to title case).
5. **Handling outliers**: Removed rows where `Age` was below a certain threshold.
6. **Renaming columns**: Renamed columns for better clarity.

This covers essential data cleaning operations using Pandas!
