# Learn Python 101

### Set custom NA values
```python
# Create dict specifying that 0s in zipcode are NA values
null_values = {'zipcode':0}

# Load csv using na_values keyword argument
data = pd.read_csv("vt_tax_data_2016.csv", 
                   na_values=null_values)

# View rows with NA ZIP codes
print(data[data.zipcode.isna()])
```

### Skip bad data
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

### Load a portion of excel only
```python
# Create string of lettered columns to load
col_string = "AD, AW:BA"

# Load data with skiprows and usecols set
survey_responses = pd.read_excel("fcc_survey_headers.xlsx", 
                                 skiprows=2, 
                                 usecols=col_string)
```

### Combine worksheets in Excel workbook
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

### Append multiple worksheets to single Dataframe
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

### Set custom TRUE/FALSE values

```python
# Load file with Yes as a True value and No as a False value
survey_subset = pd.read_excel("fcc_survey_yn_data.xlsx",
                              dtype={"HasDebt": bool,
                              "AttendedBootCampYesNo": bool},
                              true_values=["Yes"],                      
                              false_values=["No"])
```

