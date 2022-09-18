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
- `df.to_csv(file_name)`: Save data to csv (`file_name` should end with '.csv)
  - `df.to_csv(file_name, index=False)`: do not include  the row index numbers in the file
  - `df.to_excel(file_name)`: save to excel
  - `df.csv(file_name, sep='\t')`: save to csv using tabs (`\t`)as the delimeter (`sep`)
# Examples

## Get the total for the numeric values and add it as a new 'Total' column
```python
df['Total'] = df['HP'] + df['Attack'] + df['Defense'] + df['Sp.Atk'] + df['Sp. Def'] + df['Speed']
```

### Without using string liteals, get the total for the numeric values and add it as a new 'Total' column
```python
# Get the values every row (:) 
#    for columns 4 - 9 (4:9). 
#    Sum the values for each row (axis=1) 
#    and save it in the new column 'Total'
df['Total'] = df.iloc[:, 4:9].sum(axis=1)
```

## Reordering columns: move the "total" column between 'legendary' and 'HP'
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

# Advanced
## Filtering Rows
```python
# Get all the pokemon whose type 1 is grass AND type 2 is poison
df.loc[(df['Type 1'] == 'Grass') & (df['Type 2'] == 'Poison')]

# Get all the pokemon whose type 1 is grass OR type 2 is poison
df.loc[(df['Type 1'] == 'Grass') | (df['Type 2'] == 'Poison')]

# Note: if you don't include the parenthesis around each equality expression then
# it will evaluate the inner 'or' operands first
# df.loc[df['Type 1'] == 'Grass' | df['Type 2'] == 'Poison']
# is equal to
# df.loc[df['Type 1'] == ('Grass' | df['Type 2']) == 'Poison']

# pulling the expressions in seperate variables makes it easier to read and 
# reduces the pobability of errors
grass_type1_pokemon = df['Type 1'] == 'Grass'
poison_type2_pokemon = df['Type 2'] == 'Poison'
df.loc[grass_type1_pokemon | poison_type2_pokemon]
```


## Filter out all of the Names that contain 'Mega'
```python
# Rows whose Names value contain the string 'Mega'
names_that_contain_mega = df['Name'].str.contains('Mega')

# In dataframes, '~' means 'not'
names_that_donot_contain_mega = ~df['Name'].str.contains('Mega') # could also do `~names_that_contain_mega`

# Data frame containing rows for which their Name column value doesn't contain the string 'Mega'
df.loc[~df['Name'].str.contains('Mega')] # or `df.loc[names_that_donot_contain_mega]`
```


## Find all of the rows for which Type 1 is 'Fire' or 'Grass'
```python
df.loc[(df['Type 1'].str.contains('Fire')) | (df['Type 1'].str.contains('Grass'))]

# Here, we're using a regex expression to accomplish the same thing
# In regex, '|' means 'or'
# Regexes are powerful but complicated, explore some basic 
# regex symbols here https://www.codexpedia.com/regex/regex-symbol-list-and-regex-examples/
df.loc[df['Type 1'].str.contains('Fire|Grass', regex=True)]

# We can also make the `contains` check ignore case by using `re.I` (the ignore case flag)
# 're' is a standard python library containing regex matching options
# Read more about it here https://docs.python.org/3/library/re.html
import re
df.loc[df['Type 1'].str.contains('fire|grass', regex=True, flags=re.I)]
```

## Find all of the pokemon whose Names start with 'pi' (upper or lower case)
```python
# Read more about it here https://docs.python.org/3/library/re.html
import re

# These are all the names that contain pi
df.loc[df['Name'].str.contains('pi')]

# These are all the names that contain pi (as a regex)
# In regex
#    [] means a range of items
#     A-z means the characters between A and z in ASCII (A = 65, z = 122, all upper and lower case characters fall between this range)
#    * means any (0 or more) of the preceeding values
#    [a-Z]* means a string that has zero or more letters
#    pi[a-Z]* means a string that has the letter 'p' followed by 'i' followed by zero or more other letters
# This would also return pokemon names that 'contain' the letters p and i, not just starts with
df.loc[df['Name'].str.contains('pi[A-z]*', regex=True, flags=re.I)]

# In regex, '^' means starts with/ the beginning of the line
#    ^pi[a-Z]* means a string that starts with the letter 'p' followed by 'i' followed by zero or more letters
df.loc[df['Name'].str.contains('^pi[A-z]*', regex=True, flags=re.I)]
```

## Updating indexes on new dataframes
```python
# Get all the pokemon whose type 1 is grass AND type 2 is poison AND
# has more than 70 hit points
new_df = df.loc[(df['Type 1'] == 'Grass') & (df['Type 2'] == 'Poison') & (df['HP'] > 70)]

# Note that the type of new_df is a data frame, just like df
# We can check the type like so
type(df) # >> pandas.core.frame.DataFrame
type(new_df) # >> pandas.core.frame.DataFrame

# `reset_index` will create a new column with the updated indexes, but keep the 
# old index column
new_df = new_df.reset_index() # Note: This also returns a new data frame, that's why we have to update the variable

# This will create a new column with updated indexes and drop
# the old column
new_df = new_df.reset_index(drop=True)

# This will create a new column with updated indexes, drop
# the old column, AND save the resulting data frame in the
# original variable
new_df.reset_index(drop=True, inplace=True)
```

# Conditional Changes

## Change the Type 1 of all pokemon whose Type 1 is 'Fire' to 'Flamer'
```python
# Rows whose Type 1 value is Fire
df['Type 1'] == 'Fire'

# Get the Type 1 column for all (rows whose Type 1 value is Fire)
df.loc[df['Type 1'] == 'Fire', 'Type 1']

# Change the value of (the Type 1 column for all (rows whose Type 1 value is Fire)) to 'Flamer'
df.loc[df['Type 1'] == 'Fire', 'Type 1'] = "Flamer"
```

## Set the Legendary column to True for all pokemon whose Type 1 is 'Grass'
```python
df.loc[df['Type 1'] == 'Grass', 'Legendary'] = True
```

## If the Total column is greater than 500, set Generation and Legendary columns to True
```python
df.loc[df['Total'] > 500, ['Generation','Legendary']] = True
```

# Aggregate Statistics

## Count the number of each filled in column value for every pokemon per kind of Type 1 
```python
df.groupby(['Type 1']).count()
```

## Count the number of each filled in column value for every pokemon per kind of Type 1 and show only the 'count' column
```python
# We need to inialize the 'count' column in order to access it later
df['count'] = 1

df.groupby(['Type 1']).count()['count']
```

## For each kind of Type 1, show the find the number of every kind of Type 2
```python
# We need to inialize the 'count' column in order to access it later
df['count'] = 1

df.groupby(['Type 1', 'Type 2']).count()['count']
```

## Get the averages for the data set grouped by Type 1 pokemon sorted from highest to lowest by average 'Defense'
```python
# Group the data by Type 1 and calculate the mean for each group
df.groupby(['Type 1']).mean()

# Now, sort the values by 'Defense'
df.groupby(['Type 1']).mean().sort_values('Defense')

# Finally, show the values sorted by 'Defense' from highest to lowest
df.groupby(['Type 1']).mean().sort_values('Defense', ascending=False)
```

# Reading large datasets
## Only load 5 rows at a time
```python
for df in pd.read_csv('pokemon_data.csv', chunksize=5):
  # Perform you operations on each dataframe (ie: set of 5 rows) here
  print(df)
```

# Joining Data Frames
```python
# Create a new data frame with column names equivalent to our existing 
# data frame's columns (it will no have any rows/data, only columns)
new_df = pd.DataFrame(columns = df.columns)

# For every 5 rows we read from the file
for df in pd.read_csv('pokemon_data.csv', chunksize=5):
  # Count the number of non-blank Type 1 values
  results = df.groupby(['Type 1']).count()

  # Add the results to our new_df
  new_df = pd.concat([new_df, results])
```