<a href="https://www.kaggle.com/code/wuttipats/us-birth-rates-eda-with-sql?scriptVersionId=131569636" target="_blank"><img align="left" alt="Kaggle" title="Open in Kaggle" src="https://kaggle.com/static/images/open-in-kaggle.svg"></a>

# US birth rates EDA with SQL 👶

<span style="color: gray">"Exploratory data analysis on  US birth data using Structured Query Language (SQL)"</span>

* **Created By:** Wuttipat S.
* **Created Date:** 2023-05-30
* **Status:** <span style="background-color: green; color: white; font-weight: bold">Completed</span>


![newborn1200x500.png](https://export-download.canva.com/oTei4/DAFkXvoTei4/2/0/0001-5733870933.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJHKNGJLC2J7OGJ6Q%2F20230529%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230529T165322Z&X-Amz-Expires=55212&X-Amz-Signature=b7297875d87eb995f83684e45b2a24fc2092725e0a3ffc4474556934f5d4b189&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%2A%3DUTF-8%27%27newborn1200x500.png&response-expires=Tue%2C%2030%20May%202023%2008%3A13%3A34%20GMT)
> photo by canva

# Notebook Odjective
---
This notebook purpose is using **Structured Query Language(SQL)** to perform these analysis tasks as much as possible:
- Data Cleaning
- Data Validation
- Exploratory Data Analysis (EDA)
- Summary an insights

# About Dataset
---
## Introduction
This dataset provides birth rates and related data across the 50 states and DC from 2016 to 2021. The data was sourced from the Centers for Disease Control and Prevention (CDC) and includes detailed information such as number of births, gender, birth weight, state, and year of the delivery. A particular emphasis is given to detailed information on the mother's educational level. With this dataset, one can, for example, examine trends and patterns in birth rates across different academic groups and geographic locations.


### Origin
The data in this dataset was obtained using CDC's WONDER retrieval tool on the CDC Natality page

### Column Descriptions
* State ➡️ state name in full (includes District of Columbia)
* State Abbreviation ➡️ 2-character state abbreviation
* Year ➡️ 4-digit year
* Gender ➡️ Gender of baby
* Education Level of Mother ➡️ See table below
* Education Level Code ➡️ See table below
* Number of Births ➡️ Number of births for the category
* Average Age of Mother (years) ➡️ Mother's average age in the category
* Average Birth Weight (g) ➡️ Average birth weight in the category

### Education levels and codes used in dataset
Code	Mother's Education Level
* 1	8th grade or less
* 2	9th through 12th grade with no diploma
* 3	High school graduate or GED completed
* 4	Some college credit, but not a degree
* 5	Associate degree (AA, AS)
* 6	Bachelor's degree (BA, AB, BS)
* 7	Master's degree (MA, MS, MEng, MEd, MSW, MBA)
* 8	Doctorate (PhD, EdD) or Professional Degree (MD, DDS, DVM, LLB, JD)
* -9	Unknown or Not Stated


![](https://www.pngitem.com/pimgs/b/508-5089961_text-decoration-png.png)

# Step 1: Import the Required Libraries


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
plt.style.use('ggplot')

```

# Step 2: Load the Dataset


```python
df = pd.read_csv('/kaggle/input/temporary-us-births/us_births_2016_2021.csv')
display(df.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>State Abbreviation</th>
      <th>Year</th>
      <th>Gender</th>
      <th>Education Level of Mother</th>
      <th>Education Level Code</th>
      <th>Number of Births</th>
      <th>Average Age of Mother (years)</th>
      <th>Average Birth Weight (g)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>8th grade or less</td>
      <td>1</td>
      <td>1052</td>
      <td>27.8</td>
      <td>3116.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>9th through 12th grade with no diploma</td>
      <td>2</td>
      <td>3436</td>
      <td>24.1</td>
      <td>3040.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>High school graduate or GED completed</td>
      <td>3</td>
      <td>8777</td>
      <td>25.4</td>
      <td>3080.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>Some college credit, but not a degree</td>
      <td>4</td>
      <td>6453</td>
      <td>26.7</td>
      <td>3121.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>Associate degree (AA, AS)</td>
      <td>5</td>
      <td>2227</td>
      <td>28.9</td>
      <td>3174.3</td>
    </tr>
  </tbody>
</table>
</div>


# Step 3: Create a SQLite Database
Now, let's create a SQLite database and use pandas DataFrame `to_sql()` method to write records stored in a DataFrame to our SQLite database.


```python
import sqlite3

# Create a new SQLite database 
conn = sqlite3.connect('births.db')

# Write the data to a SQLite table name 'births'
df.to_sql('births', conn, if_exists='replace', index=False)

```




    5496



# Step 4: Test Connection and SQL Query
Make a test to ensure everything works fine by executing a simple SQL query.


```python
test_df = pd.read_sql('SELECT * FROM births LIMIT 5;', conn)
test_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>State Abbreviation</th>
      <th>Year</th>
      <th>Gender</th>
      <th>Education Level of Mother</th>
      <th>Education Level Code</th>
      <th>Number of Births</th>
      <th>Average Age of Mother (years)</th>
      <th>Average Birth Weight (g)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>8th grade or less</td>
      <td>1</td>
      <td>1052</td>
      <td>27.8</td>
      <td>3116.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>9th through 12th grade with no diploma</td>
      <td>2</td>
      <td>3436</td>
      <td>24.1</td>
      <td>3040.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>High school graduate or GED completed</td>
      <td>3</td>
      <td>8777</td>
      <td>25.4</td>
      <td>3080.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>Some college credit, but not a degree</td>
      <td>4</td>
      <td>6453</td>
      <td>26.7</td>
      <td>3121.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alabama</td>
      <td>AL</td>
      <td>2016</td>
      <td>F</td>
      <td>Associate degree (AA, AS)</td>
      <td>5</td>
      <td>2227</td>
      <td>28.9</td>
      <td>3174.3</td>
    </tr>
  </tbody>
</table>
</div>



# Step 5: Data Preparation

> **Note**: 
> - `pd.read_sql()` is designed for executing `SELECT` statements and returning the result as a DataFrame. It's not designed to handle data manipulation language (DML) statements such as UPDATE.
> - To execute an `UPDATE`, `INSERT` or `DELETE` statement, you need to use the appropriate function from your database connector (sqlite3, psycopg2, etc.).
> - This require to connection only one times.

First, a cursor object is created using the `cursor()` method of the database connection `conn`. 


```python
cursor = conn.cursor() # Run this once is enough
```

A series of `ALTER TABLE` statements are executed using the `execute()` method of the cursor object. 


```python
cursor.execute('ALTER TABLE births RENAME COLUMN "State" TO "state"')
cursor.execute('ALTER TABLE births RENAME COLUMN "State Abbreviation" TO "state_abbreviation"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Year" TO "year"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Gender" TO "gender"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Education Level of Mother" TO "education_level_of_mother"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Education Level Code" TO "education_level_code"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Number of Births" TO "number_of_births"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Average Age of Mother (years)" TO "average_age_of_mother_years"')
cursor.execute('ALTER TABLE births RENAME COLUMN "Average Birth Weight (g)" TO "average_birth_weight_g"')

conn.commit()
```

An `UPDATE` statement is executed using the `execute()` method of the cursor object.


```python
original = pd.read_sql('SELECT DISTINCT education_level_of_mother FROM births', conn)
```


```python
cursor.execute('''
UPDATE births
SET education_level_of_mother = 
    CASE
        WHEN education_level_of_mother = 'Doctorate (PhD, EdD) or Professional Degree (MD, DDS, DVM, LLB, JD)' THEN 'Doctorate or Professional Degree'
        WHEN education_level_of_mother = 'Master''s degree (MA, MS, MEng, MEd, MSW, MBA)' THEN 'Master''s Degree'
        WHEN education_level_of_mother = 'Bachelor''s degree (BA, AB, BS)' THEN 'Bachelor''s Degree'
        WHEN education_level_of_mother = 'Associate degree (AA, AS)' THEN 'Associate Degree'
        WHEN education_level_of_mother = 'Some college credit, but not a degree' THEN 'Some College Credit'
        WHEN education_level_of_mother = 'High school graduate or GED completed' THEN 'High School Graduate'
        WHEN education_level_of_mother = '9th through 12th grade with no diploma' THEN '9th-12th Grade with No Diploma'
        WHEN education_level_of_mother = '8th grade or less' THEN '8th Grade or Less'
        WHEN education_level_of_mother = 'Unknown or Not Stated' THEN 'Unknown'
        ELSE education_level_of_mother
    END;
    ''')
conn.commit()

```


```python
tranformed = pd.read_sql('SELECT DISTINCT education_level_of_mother FROM births', conn)
```


```python
pd.DataFrame({'original': original.education_level_of_mother, 'tranformed': tranformed.education_level_of_mother})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>original</th>
      <th>tranformed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8th grade or less</td>
      <td>8th Grade or Less</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9th through 12th grade with no diploma</td>
      <td>9th-12th Grade with No Diploma</td>
    </tr>
    <tr>
      <th>2</th>
      <td>High school graduate or GED completed</td>
      <td>High School Graduate</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Some college credit, but not a degree</td>
      <td>Some College Credit</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Associate degree (AA, AS)</td>
      <td>Associate Degree</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Bachelor's degree (BA, AB, BS)</td>
      <td>Bachelor's Degree</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Master's degree (MA, MS, MEng, MEd, MSW, MBA)</td>
      <td>Master's Degree</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Doctorate (PhD, EdD) or Professional Degree (M...</td>
      <td>Doctorate or Professional Degree</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Unknown or Not Stated</td>
      <td>Unknown</td>
    </tr>
  </tbody>
</table>
</div>



![](https://www.pngitem.com/pimgs/b/508-5089961_text-decoration-png.png)

# Step 6: Data Cleaning & Data Validation
- Check *table info*
- Check *missing* values and remove if it exist.
- Check *duplicated* values and remove if it exist.
- Check *outlier, data types* and *spelling.*

### Table Info


```python
# Get number of rows
query = "SELECT COUNT(*) as num FROM births;"
result = pd.read_sql(query, conn)
number_of_rows = result['num'].values[0]

# Get number of columns
query = "PRAGMA table_info(births);"
result = pd.read_sql(query, conn)
number_of_columns = len(result)

print(f"The table 'births' has {number_of_rows} rows and {number_of_columns} columns.")

```

    The table 'births' has 5496 rows and 9 columns.
    


```python
query = "PRAGMA table_info(births);"
tables_info = pd.read_sql(query, conn,)
tables_info
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cid</th>
      <th>name</th>
      <th>type</th>
      <th>notnull</th>
      <th>dflt_value</th>
      <th>pk</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>state</td>
      <td>TEXT</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>state_abbreviation</td>
      <td>TEXT</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>year</td>
      <td>INTEGER</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>gender</td>
      <td>TEXT</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>education_level_of_mother</td>
      <td>TEXT</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>education_level_code</td>
      <td>INTEGER</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>number_of_births</td>
      <td>INTEGER</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>average_age_of_mother_years</td>
      <td>REAL</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>average_birth_weight_g</td>
      <td>REAL</td>
      <td>0</td>
      <td>None</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Check missing values


```python
missing = pd.read_sql('''
    SELECT
    SUM(CASE WHEN state IS NULL THEN 1 ELSE 0 END) AS MissingState,
    SUM(CASE WHEN state_abbreviation IS NULL THEN 1 ELSE 0 END) AS MissingStateAbbreviation,
    SUM(CASE WHEN year IS NULL THEN 1 ELSE 0 END) AS MissingYear,
    SUM(CASE WHEN gender IS NULL THEN 1 ELSE 0 END) AS MissingGender,
    SUM(CASE WHEN education_level_of_mother IS NULL THEN 1 ELSE 0 END) AS MissingEducationLevelOfMother,
    SUM(CASE WHEN education_level_code IS NULL THEN 1 ELSE 0 END) AS MissingEducationLevelCode,
    SUM(CASE WHEN number_of_births IS NULL THEN 1 ELSE 0 END) AS MissingNumberOfBirths,
    SUM(CASE WHEN average_age_of_mother_years IS NULL THEN 1 ELSE 0 END) AS MissingAverageAgeOfMotherYears,
    SUM(CASE WHEN average_birth_weight_g IS NULL THEN 1 ELSE 0 END) AS MissingAverageBirthWeightG
    FROM births;
    ''', conn)

missing
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MissingState</th>
      <th>MissingStateAbbreviation</th>
      <th>MissingYear</th>
      <th>MissingGender</th>
      <th>MissingEducationLevelOfMother</th>
      <th>MissingEducationLevelCode</th>
      <th>MissingNumberOfBirths</th>
      <th>MissingAverageAgeOfMotherYears</th>
      <th>MissingAverageBirthWeightG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Check duplicates


```python
duplicates = pd.read_sql('''
SELECT state, state_abbreviation, year, gender, education_level_of_mother, education_level_code, number_of_births, average_age_of_mother_years, average_birth_weight_g
FROM births
GROUP BY state, state_abbreviation, year, gender, education_level_of_mother, education_level_code, number_of_births, average_age_of_mother_years, average_birth_weight_g
HAVING COUNT(*) > 1;
''', conn)

duplicates
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>state_abbreviation</th>
      <th>year</th>
      <th>gender</th>
      <th>education_level_of_mother</th>
      <th>education_level_code</th>
      <th>number_of_births</th>
      <th>average_age_of_mother_years</th>
      <th>average_birth_weight_g</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



### Validate across every columns

- *distinct_states*


```python
query = '''
SELECT 
    COUNT(DISTINCT state) AS distinct_states,
    COUNT(DISTINCT state_abbreviation) AS distinct_state_abbreviations
FROM births;
'''
pd.read_sql(query, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>distinct_states</th>
      <th>distinct_state_abbreviations</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>51</td>
      <td>51</td>
    </tr>
  </tbody>
</table>
</div>



* *year*


```python
query = '''SELECT DISTINCT year FROM births'''
display(pd.read_sql(query, conn))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021</td>
    </tr>
  </tbody>
</table>
</div>


- *gender*


```python
query = '''SELECT DISTINCT gender FROM births'''
display(pd.read_sql(query, conn))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M</td>
    </tr>
  </tbody>
</table>
</div>


- *education_level_of_mother*


```python
query = '''SELECT DISTINCT education_level_of_mother, education_level_code FROM births'''
pd.read_sql(query, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>education_level_of_mother</th>
      <th>education_level_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8th Grade or Less</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9th-12th Grade with No Diploma</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>High School Graduate</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Some College Credit</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Associate Degree</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Bachelor's Degree</td>
      <td>6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Master's Degree</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Doctorate or Professional Degree</td>
      <td>8</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Unknown</td>
      <td>-9</td>
    </tr>
  </tbody>
</table>
</div>



- *number_of_births*


```python
query = '''
SELECT 
    MIN(number_of_births),
    MAX(number_of_births)
FROM births;
'''
pd.read_sql(query, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MIN(number_of_births)</th>
      <th>MAX(number_of_births)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10</td>
      <td>59967</td>
    </tr>
  </tbody>
</table>
</div>



- *average_age_of_mother_years*


```python
query = '''
SELECT 
    MIN(average_age_of_mother_years),
    MAX(average_age_of_mother_years)
FROM births;
'''
pd.read_sql(query, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MIN(average_age_of_mother_years)</th>
      <th>MAX(average_age_of_mother_years)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>23.1</td>
      <td>35.5</td>
    </tr>
  </tbody>
</table>
</div>



- *average_birth_weight_g*


```python
query = '''
SELECT 
    MIN(average_birth_weight_g),
    MAX(average_birth_weight_g)
FROM births;
'''
pd.read_sql(query, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MIN(average_birth_weight_g)</th>
      <th>MAX(average_birth_weight_g)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2451.9</td>
      <td>3585.7</td>
    </tr>
  </tbody>
</table>
</div>



> After we cleaning and validating data this is result:
> - The dataset has 5496 rows and 9 columns.
> - Checking each columns

|   | Column name                 | Data type | Missing value | Details                      |
|---|-----------------------------|-----------|---------------|------------------------------|
| 0 |                       state |      TEXT |          None | 51 states                    |
| 1 |          state_abbreviation |      TEXT |          None | 51 states                    |
| 2 |                        year |   INTEGER |          None | Form 2016-2021               |
| 3 |                      gender |      TEXT |          None | Male or Female               |
| 4 |   education_level_of_mother |      TEXT |          None | 9 indentical levels          |
| 5 |        education_level_code |   INTEGER |          None | From 1 to 8, (-9) as unknown |
| 6 |            number_of_births |   INTEGER |          None | Range 10 to 59967            |
| 7 | average_age_of_mother_years |      REAL |          None | Range 23.1 to 35.5           |
| 8 |      average_birth_weight_g |      REAL |          None | Range 2451.9 to 3585.7       |

![](https://www.pngitem.com/pimgs/b/508-5089961_text-decoration-png.png)

# Step 7: Start Exploratory Data Analysis

1. What is the total number of birth across years between male and female baby?
1. What is distribution of weight across gender?
1. What is the average birth weight across education level of mother?
1. Which state have highest births across year?
1. What is the average age of mothers per education level?


### 1. What is the average birth rate across years between male and female baby?



```python
query = 'SELECT year, gender, SUM(number_of_births) FROM births GROUP BY year, gender'
sum_birth_across_year = pd.read_sql(query, conn)
sum_birth_across_year
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>gender</th>
      <th>SUM(number_of_births)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016</td>
      <td>F</td>
      <td>1927676</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016</td>
      <td>M</td>
      <td>2018177</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2017</td>
      <td>F</td>
      <td>1882608</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2017</td>
      <td>M</td>
      <td>1972871</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018</td>
      <td>F</td>
      <td>1853528</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018</td>
      <td>M</td>
      <td>1938179</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019</td>
      <td>F</td>
      <td>1830085</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019</td>
      <td>M</td>
      <td>1917446</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2020</td>
      <td>F</td>
      <td>1765547</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2020</td>
      <td>M</td>
      <td>1848086</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2021</td>
      <td>F</td>
      <td>1790870</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2021</td>
      <td>M</td>
      <td>1873407</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.lineplot(data=sum_birth_across_year, x='year', y='SUM(number_of_births)', hue='gender', marker='o', linestyle='dashed')
plt.xlabel('Year')
plt.ylabel('Number of Births')
plt.title('Total Number of Births Across Years')
plt.show()
```


    
![png](us-birth-rates-eda-with-sql_files/us-birth-rates-eda-with-sql_54_0.png)
    


> - From the plot, there are negative correlation between number of births and year. 
> - Althrough in 2021 the number slightly going up, but overall this is downside trend.

### 2. What is distribution of weight across gender?


```python
query = '''
SELECT gender, average_birth_weight_g
FROM births
'''

avg_weight = pd.read_sql(query, conn)
avg_weight
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gender</th>
      <th>average_birth_weight_g</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F</td>
      <td>3116.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>F</td>
      <td>3040.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>F</td>
      <td>3080.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>F</td>
      <td>3121.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>F</td>
      <td>3174.3</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5491</th>
      <td>M</td>
      <td>3261.1</td>
    </tr>
    <tr>
      <th>5492</th>
      <td>M</td>
      <td>3286.0</td>
    </tr>
    <tr>
      <th>5493</th>
      <td>M</td>
      <td>3249.3</td>
    </tr>
    <tr>
      <th>5494</th>
      <td>M</td>
      <td>3262.0</td>
    </tr>
    <tr>
      <th>5495</th>
      <td>M</td>
      <td>3177.5</td>
    </tr>
  </tbody>
</table>
<p>5496 rows × 2 columns</p>
</div>




```python
sns.histplot(data=avg_weight, hue='gender', x='average_birth_weight_g')

plt.xlabel('Average Birth Weight (g)')
plt.ylabel('Count')
plt.legend(title='Gender')
plt.title('Distribution of Average Birth Weight by Gender')

# Display the plot
plt.show()

```


    
![png](us-birth-rates-eda-with-sql_files/us-birth-rates-eda-with-sql_58_0.png)
    



> - The histogram reveals that the average weight of newborns follows a roughly normal distribution for both males and females. 
> - However, upon closer examination, the graph exhibits a slight left skew, suggesting that underweight babies are born more frequently than overweight ones.

### 3. What is the average birth weight across education level of mother?




```python
avg_birth_weight_across_mother_edu = pd.read_sql('''
    SELECT education_level_of_mother, gender, average_birth_weight_g
    FROM births
    WHERE education_level_of_mother != 'Unknown'
    ORDER BY education_level_code DESC
''', conn)

avg_birth_weight_across_mother_edu.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>education_level_of_mother</th>
      <th>gender</th>
      <th>average_birth_weight_g</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Doctorate or Professional Degree</td>
      <td>F</td>
      <td>3196.7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Doctorate or Professional Degree</td>
      <td>M</td>
      <td>3368.4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Doctorate or Professional Degree</td>
      <td>F</td>
      <td>3230.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Doctorate or Professional Degree</td>
      <td>M</td>
      <td>3313.8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Doctorate or Professional Degree</td>
      <td>F</td>
      <td>3242.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(10, 8)) 

sns.boxplot(
    data=avg_birth_weight_across_mother_edu,
    x='average_birth_weight_g',
    y='education_level_of_mother',
    hue='gender'
)

plt.xlabel('Average Birth Weight (grams)', fontsize=14)
plt.ylabel('Education Level', fontsize=14)
plt.title('Average Birth Weight Across Mother Education Levels', fontsize=16)


plt.show()

```


    
![png](us-birth-rates-eda-with-sql_files/us-birth-rates-eda-with-sql_62_0.png)
    


> - From this visualize, higher education have trend to giving birth heavier baby. While boy having more average weight than a girl since this is normal.

### 4. Which state have highest births rate across year? and which state have lowest?
Determining the states with the highest and lowest birth rates across the years can be challenging due to the need to compare birth rates relative to the population of each state. Simply summarizing the total count of births is not sufficient because larger states tend to have higher numbers of births naturally, given their larger populations.

To achieve this, the following steps can be followed:
1. Bring external information in this case I use data from www.statsamerica.org about us population in 2022
1. Create new table and insert data into it.
1. Join new table with dataset we have
1. Create new columns calculate birth rate per 100k population.

> Note: The external data have number of population in 2022, which not same years as our dataset this can lead to bias and unaccurate.

By combining these two statements, we ensure that the `'us_pop'` table is dropped if it exists, providing a fresh start, and then recreated with the desired column structure. This approach allows us to begin populating the `'us_pop'` table with accurate and up-to-date population data for further analysis and exploration


```python
# Check whether table 'us_pop' already created, if it existed delete it.
cursor.execute('DROP TABLE IF EXISTS us_pop;') 

# Create empty table'us_pop'
cursor.execute('''
CREATE TABLE us_pop (
    state TEXT,
    population INTEGER);
''')

# Write SQL to INSERT data into 'us_pop'
query = '''
INSERT INTO us_pop (state, population)
VALUES
    ('California', 39029342),
    ('Texas', 30029572),
    ('Florida', 22244823),
    ('New York', 19677151),
    ('Pennsylvania', 12972008),
    ('Illinois', 12582032),
    ('Ohio', 11756058),
    ('Georgia', 10912876),
    ('North Carolina', 10698973),
    ('Michigan', 10034113),
    ('New Jersey', 9261699),
    ('Virginia', 8683619),
    ('Washington', 7785786),
    ('Arizona', 7359197),
    ('Tennessee', 7051339),
    ('Massachusetts', 6981974),
    ('Indiana', 6833037),
    ('Missouri', 6177957),
    ('Maryland', 6164660),
    ('Wisconsin', 5892539),
    ('Colorado', 5839926),
    ('Minnesota', 5717184),
    ('South Carolina', 5282634),
    ('Alabama', 5074296),
    ('Louisiana', 4590241),
    ('Kentucky', 4512310),
    ('Oregon', 4240137),
    ('Oklahoma', 4019800),
    ('Connecticut', 3626205),
    ('Utah', 3380800),
    ('Iowa', 3200517),
    ('Nevada', 3177772),
    ('Arkansas', 3045637),
    ('Mississippi', 2940057),
    ('Kansas', 2937150),
    ('New Mexico', 2113344),
    ('Nebraska', 1967923),
    ('Idaho', 1939033),
    ('West Virginia', 1775156),
    ('Hawaii', 1440196),
    ('New Hampshire', 1395231),
    ('Maine', 1385340),
    ('Montana', 1122867),
    ('Rhode Island', 1093734),
    ('Delaware', 1018396),
    ('South Dakota', 909824),
    ('North Dakota', 779261),
    ('Alaska', 733583),
    ('District of Columbia', 671803),
    ('Vermont', 647064),
    ('Wyoming', 581381);
'''

# Run the query we just write.
cursor.execute(query)

# Commit the changes to the database
conn.commit()
```

#### 4.1 Which state have highest births rate across year?


```python
query = '''
SELECT year, state, MAX(births_per_100k) AS highest_births_per_100k
FROM (SELECT births.year, births.state, us_pop.population, (SUM(births.number_of_births) * 100000 / us_pop.population) AS 'births_per_100k'
        FROM births
        LEFT JOIN us_pop ON births.state = us_pop.state
        GROUP BY births.year, births.state)
GROUP BY year;
'''

highest_birth_rate_state_across_year = pd.read_sql(query, conn)

display(highest_birth_rate_state_across_year)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>highest_births_per_100k</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016</td>
      <td>Alaska</td>
      <td>1527</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017</td>
      <td>Utah</td>
      <td>1437</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018</td>
      <td>Utah</td>
      <td>1396</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019</td>
      <td>Utah</td>
      <td>1385</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020</td>
      <td>Utah</td>
      <td>1351</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021</td>
      <td>Utah</td>
      <td>1381</td>
    </tr>
  </tbody>
</table>
</div>


#### 4.2 Which state have lowest births rate across year?


```python
query = '''
SELECT year, state, MIN(births_per_100k) AS highest_births_per_100k
FROM (
        SELECT 
            births.year,
            births.state, us_pop.population,
            (SUM(births.number_of_births) * 100000 / us_pop.population) AS 'births_per_100k'
        FROM births
        LEFT JOIN us_pop ON births.state = us_pop.state
        GROUP BY births.year, births.state
    )
GROUP BY year;
'''

lowest_birth_rate_state_across_year = pd.read_sql(query, conn)

display(lowest_birth_rate_state_across_year)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>highest_births_per_100k</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016</td>
      <td>New Hampshire</td>
      <td>879</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017</td>
      <td>New Hampshire</td>
      <td>868</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018</td>
      <td>Vermont</td>
      <td>839</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019</td>
      <td>Vermont</td>
      <td>828</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020</td>
      <td>Vermont</td>
      <td>793</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021</td>
      <td>Vermont</td>
      <td>832</td>
    </tr>
  </tbody>
</table>
</div>


> - From query result said *Alaska* and *Utah* have highest number of new born, while New *Hampshire* and *Vermont* have lowest. 

### 5. What is the average age of mothers across education level?


```python
query = '''
    SELECT education_level_of_mother, AVG(average_age_of_mother_years)
    FROM births
    WHERE education_level_of_mother != 'Unknown'
    GROUP BY education_level_of_mother
    ORDER BY `education_level_code` DESC
'''
avg_age = pd.read_sql(query, conn)

display(avg_age)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>education_level_of_mother</th>
      <th>AVG(average_age_of_mother_years)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Doctorate or Professional Degree</td>
      <td>33.698039</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Master's Degree</td>
      <td>32.781699</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bachelor's Degree</td>
      <td>31.230229</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Associate Degree</td>
      <td>29.861438</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Some College Credit</td>
      <td>28.068627</td>
    </tr>
    <tr>
      <th>5</th>
      <td>High School Graduate</td>
      <td>26.491993</td>
    </tr>
    <tr>
      <th>6</th>
      <td>9th-12th Grade with No Diploma</td>
      <td>25.064379</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8th Grade or Less</td>
      <td>29.410621</td>
    </tr>
  </tbody>
</table>
</div>



```python
# Set the figure size
plt.figure(figsize=(8, 5))

# Create a bar plot
sns.barplot(data=avg_age, orient='h',
            y=avg_age['education_level_of_mother'],
            x=avg_age['AVG(average_age_of_mother_years)'],
            palette="magma")

# Title and labels
plt.title('Average Age of Mothers (Order by Education Level)')
plt.xlabel('Average Age', fontsize=14)
plt.ylabel('Education Level', fontsize=14)

# Show the plot
plt.show()
```


    
![png](us-birth-rates-eda-with-sql_files/us-birth-rates-eda-with-sql_74_0.png)
    


> - We can observe a trend indicating that mothers with higher education are more likely to give birth at an older age compared to those with lower education levels. 
> - However,it is interesting to note that mothers with the lowest academic level (8th Grade or Less) tend to have a slower rate of giving birth compared to some higher education ranks.

![](https://www.pngitem.com/pimgs/b/508-5089961_text-decoration-png.png)

# Step 8: Summary & Insights

1. The data reveals an interesting trend in the number of births over the period from 2016 to 2021, indicating a downward trajectory. This decline in birth rates may be influenced by various factors. :
    * Economically, financial uncertainty and the increasing cost of raising children might be contributing to this trend
    * Societal and cultural changes have resulted in a shift towards smaller family sizes, reflecting evolving norms and attitudes towards family planning.
    * However there are surprice increase in the number of births in 2021. This is worth exploring the specific drivers behind this event.
<br><br>

1. The average weight of newborns follows a normal distribution, but with a slight left skew indicating a slightly higher frequency of underweight babies compared to overweight ones. Most babies fall within a healthy weight range according to **kidshealth.org.**
    * The information from kidshealth.org supports this observation by stating that newborns come in a range of healthy sizes, with most babies born between 5 pounds, 8 ounces (2,500 grams) and 8 pounds, 13 ounces (4,000 grams). Babies weighing below or above this range are still generally considered fine and healthy.
<br><br>

1. There is a positive correlation between maternal education level and birth weight. Higher education levels are associated with better access to healthcare and nutrition, leading to healthier birth weights. The observed trends can cause by following factors:
<br><br>

1. Alaska and Utah have the highest number of newborns, while New Hampshire and Vermont have the lowest. The reasons for these variations may need further analysis.
<br><br>

1. Mothers with higher education levels tend to give birth at older ages, in line with the trend of prioritizing career and personal goals before starting a family.
