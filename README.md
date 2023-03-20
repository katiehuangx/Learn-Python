# Learn Python 101

## Set custom NA values
```python
# Create dict specifying that 0s in zipcode are NA values
null_values = {'zipcode':0}

# Load csv using na_values keyword argument
data = pd.read_csv("vt_tax_data_2016.csv", 
                   na_values=null_values)

# View rows with NA ZIP codes
print(data[data.zipcode.isna()])
```

## Skip bad data
```python
try:
  # Set warn_bad_lines to issue warnings about bad records
  data = pd.read_csv("vt_tax_data_2016_corrupt.csv", 
                     error_bad_lines=False, 
                     warn_bad_lines=True)
  
  # View first 5 records
  print(data.head())
  
except pd.errors.ParserError:
    print("Your data contained rows that could not be parsed.")
```

Load TSV file
```python
# Load TSV using the sep keyword argument to set delimiter
data = pd.read_csv("vt_tax_data_2016.tsv", sep='\t')

# Plot the total number of tax returns by income group "N1"
counts = data.groupby("agi_stub").N1.sum()
counts.plot.bar()
plt.show()
```

## Load a portion of excel only
```python
# Create string of lettered columns to load
col_string = "AD, AW:BA"

# Load data with skiprows and usecols set
survey_responses = pd.read_excel("fcc_survey_headers.xlsx", 
                                 skiprows=2, 
                                 usecols=col_string)
```

## Combine worksheets in Excel workbook
```python
# Create empty dataframe to hold all loaded sheets
combined_df = pd.DataFrame()

# Iterate through dataframes in dictionary
for sheet_name, frame in df.items():
  # Add a column so we know which year data is from
  frame["Year"] = sheet_name
  
  # Add each dataframe to df
  combined_df = df.append(frame)
```

```python
# Load both the 2016 and 2017 sheets by name
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name=(["2016", "2017"]))

# Load both sheets by position and name
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name=[0, "2017"])

# Load all sheets in the Excel file
all_survey_data = pd.read_excel("fcc_survey.xlsx",
                                sheet_name=None)
```

## Append multiple worksheets to single Dataframe
```python
# Create an empty dataframe
all_responses = pd.DataFrame()

# Set up for loop to iterate through values in responses
# DataFrame.values() - Only values in the DataFrame will be returned, the axes labels will be removed.
# https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.values.html
for df in responses.values():
  # Print the number of rows being added
  print("Adding {} rows".format(df.shape[0]))
  # Append df to all_responses, assign result
  all_responses = all_responses.append(df)

# Graph employment statuses in sample
counts = all_responses.groupby("EmploymentStatus").EmploymentStatus.count()
counts.plot.barh()
plt.show()
```

## Set custom TRUE/FALSE values

```python
# Load file with Yes as a True value and No as a False value
survey_subset = pd.read_excel("fcc_survey_yn_data.xlsx",
                              dtype={"HasDebt": bool,
                              "AttendedBootCampYesNo": bool},
                              true_values=["Yes"],                      
                              false_values=["No"])
```

## Parse dates

**Using `parse_dates`**

`parse_dates` arg only works with standard datetime formats. 

```python
# List column of dates to parse
# Columns in list format
data_cols = ["start_time", "end_time", 
            [["start_time", "end_time"]]] # Combines 2 columns to be 1 column

# Columns in dictionary format
data_cols = {"start_date": "start_time",
            "end_date": "end_time",
            "start_datetime: ["start_time", "end_time"]
            }
            
# Load file, parsing standard datetime columns
df = pd.read_csv("data.csv", parse_dates=data_cols)
```

**Using `pd.to_datetime()`**

Use `pd.to_datetime()` for non-standard datetime formats. Refer to [https://strftime.org](https://strftime.org) for format.

<img width="818" alt="image" src="https://user-images.githubusercontent.com/81607668/226281955-76d2ac16-95d4-42f3-919b-df7982f7ec24.png">

```python
# Parse datetimes and assign result back to Part2EndTime
survey_data["Part2EndTime"] = pd.to_datetime(survey_data["Part2EndTime"], 
                                             format="%m%d%Y %H:%M:%S")
```

***

## Create databases

```python
# Load pandas and sqlalchemy's create_engine
import pandas as pd
from sqlalchemy import create_engine

# Create database engine to manage connections
engine = create_engine("sqlite:///data.db")

# View the tables in the database
print(engine.table_names())

# Load data by table name
# read_sql() accepts a string of either a SQL query to run, or a table to load.
data = pd.read_sql("data", engine)

# Load data by SQL query
query = """SELECT * FROM data;"""
data = pd.read_sql(query, engine)
```

**Load JSON files**
```python
# Load the JSON with orient specified
df = pd.read_json("dhs_report_reformatted.json", orient="split")

# Plot total population in shelters over time
df["date_of_census"] = pd.to_datetime(df["date_of_census"])
df.plot(x="date_of_census", 
        y="total_individuals_in_shelter")
plt.show()
```
