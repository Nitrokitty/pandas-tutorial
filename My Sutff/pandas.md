# Pandas

## Why pandas over excel?:
- Flexibility of python
- Excel struggles with big data

## Free datasets 
- https://www.kaggle.com/datasets

## Topics I should touch on
- saving your files in your dir
- jupyter notebook opens the dir you're currently in
- comments

## Terminology
- DataFrame: a 2-dimensional labeled data structure with columns of potentially different types. You can think of it like a spreadsheet or SQL table, or a dict of Series objects. It is generally the most commonly used pandas object.

## Pandas Functions
- `read_csv(filename)`: import csv data
  - `read_csv(filename, delimiter='\t')`: read tab seperated file
  - you can change the delimeter to what ever is seperating your columns
- `read_excel(filename)`: import excel data
### Dataframe Functions
- `df.head(x)`: return the top x number of rows
- `df.tail(x)`: return the last x number of rows
- `df.columns`: returns all of the `column_name`s
- `df[column_name]`: get all the rows of a specific `column_name`
  - `df[column_name][x:y]`: get rows x to y of a specific `column_name`
  - `df[[column_name1, column_name2, ...]]`: get all rows of the specified `column_name`s
- `df.iloc[x]`: Get row x
  - `def.iloc[x:y]`: Get rows x through y
  - `df.iloc[x,y]`: Get the value at column x row y
  - iloc stands for integer location
- for index, row in df.iterrows(): iterate through all the `row`s in the table
  - `row[column_name]`: The value of the row for the specified `column_name`
- `df.loc[df[column_name] == value]`: Return all of the rows where the value for the specified `column_name` is equal to `value`
  - find data in the dataset that's not based on indexing or names  
  - think of it as a search
- `df.describe()`: mean, standard deviation, min, max, etc stats
- `df.sort_values(column_name)`: sort rows by `column_name`
  - `df.sort_values([column_name1, column_name2], ascending=x)`: sort rows  by `column_name1`, then `column_name2`. Ascending order if `x` is `True`
- `df = df.drop(columns=[column_name])`: Remove the specified column by `column_name`


# Add a total for the numeric values
```python
df['Total'] = df['HP'] + df['Attack'] + df['Defense'] + df['Sp.Atk'] + df['Sp. Def'] + df['Speed']
# OR
# Get the values every row (:) for columns 4 - 9 (4:9). Sum the values for each row (axis=1) and save it in the new column 'Total'
df['Total'] = df.iloc[:, 4:9].sum(axis=1)
```

# Not using literal strings, sum all the numierc values
```python
columns = df.columns
df = df[columns[0:5] + [columns[-1]] + columns[4:12]]
```

# Reordering columns: move the "total" column between 'legendary' and 'HP'
```python
# Get all of the column names as a list
columns = list(df.columns)

# Reorder the column names
#   columns[0:4]: column names between indexes 0 - 4 inclusive (first 5 items)
#   columns[-1]: the last column name
#   columns[4:12]: column names between indexes 4 and 12 inclusive
new_column_order = columns[0:4] + [columns[-1]] + columns[4:12]

# Assign the new column order
df = df[new_column_order]
```

```python
```

```python
```

```python
```