import pandas as pd

# Load the dataset
df = pd.read_csv("C:/Users/hp/Downloads/marketing_campaign.csv", sep='\t')

# Handle missing values (fixing Future Warning)
df['Income'] = df['Income'].fillna(df['Income'].median())

# Convert date formats (fixing ValueError)
df['Dt_Customer'] = pd.to_datetime(df['Dt_Customer'], format='%d-%m-%Y')

# Extract year, month, day
df['Year_Customer'] = df['Dt_Customer'].dt.year
df['Month_Customer'] = df['Dt_Customer'].dt.month
df['Day_Customer'] = df['Dt_Customer'].dt.day

# Remove duplicates
df.drop_duplicates(inplace=True)

# Standardize text values
df['Education'] = df['Education'].str.lower().str.capitalize()
df['Marital_Status'] = df['Marital_Status'].replace({
    'Married': 'Married',
    'Together': 'Together',
    'Absurd': 'Other',
    'Widow': 'Widow',
    'YOLO': 'Other',
    'Divorced': 'Divorced',
    'Alone': 'Single',
    'Single': 'Single'
})

# Rename column headers
df.columns = df.columns.str.lower().str.replace(' ', '_')

# Check and fix data types
df['id'] = df['id'].astype(int)

# Summary of changes
summary = """
Dataset Cleaning Summary:

1. Handled missing values in 'Income' by filling with the median.
2. Correctly converted 'Dt_Customer' to datetime.
3. Extracted year, month, and day.
4. Removed duplicate rows.
5. Standardized 'Education' and 'Marital_Status' columns.
6. Renamed columns to lowercase with underscores.
7. Verified and adjusted data types.
"""
print(summary)

# Save the cleaned dataset
df.to_csv('cleaned_customer_data.csv', index=False)
