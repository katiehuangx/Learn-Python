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
