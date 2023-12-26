"""
Phone Number Lookup Script
--------------------------

Prerequisites:
- Python must be installed on your system.

- Pandas Library Installation:
  Pandas is required for data manipulation and analysis.
  Install it via pip with the following command:
  pip install pandas
  If you encounter any issues, visit https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html

- Openpyxl Library Installation:
  Openpyxl is used for handling Excel files.
  Install it via pip with the following command:
  pip install openpyxl
  For more details, refer to https://openpyxl.readthedocs.io/en/stable/

- Regular Expressions (re) module is used (should be available in standard Python installation).

File and Folder Locations:
- The script expects an Excel file named 'Import2.xlsx' in the same directory as the script.
- Text files with area codes (e.g., '219.txt', '812.txt', etc.) must also be in the same directory as the script.
  Each text file should contain phone numbers corresponding to its area code, one per line, formatted as 'area_code,rest_of_number'.

Functionality:
- The script reads phone numbers from the 'Phone' column of the Excel file.
- It cleans and standardizes each phone number, removing non-numeric characters and standardizing formats.
- The script checks each cleaned phone number against the numbers listed in the corresponding area code text file.
- If a match is found, the script marks the number as 'DO NOT CALL' in the 'DoNotCall' column of the Excel file.
- If no match is found, it marks the number as 'Ok to call'.
- Invalid or improperly formatted numbers are skipped and logged.
- The script saves the updated information in a new Excel file prefixed with 'updated_'.

Note:
- Ensure that the Excel file is not open in any other program while the script is running.
- Check the console output for progress updates and logs of any skipped numbers.
"""
# [Rest of the script follows]

import pandas as pd
import re

# Function to clean phone number
def clean_phone_number(number):
    # Remove non-numeric characters and leading country code if present
    cleaned_number = re.sub(r'\D', '', number)
    if len(cleaned_number) == 11 and cleaned_number.startswith('1'):
        return cleaned_number[1:]
    return cleaned_number

# Function to validate if the number is potentially valid
def is_valid_number(number):
    return number.isdigit() and (len(number) == 10 or (len(number) == 11 and number.startswith('1')))

# Load the Excel file
excel_file = 'Import2.xlsx'  # Replace with your Excel file path
df = pd.read_excel(excel_file)

# Function to load numbers from a text file
def load_numbers_from_file(file_path):
    with open(file_path, 'r') as file:
        return set(file.read().splitlines())

# Load numbers from text files
numbers_by_area_code = {
    '219': load_numbers_from_file('219.txt'),
    '812': load_numbers_from_file('812.txt'),
    '317': load_numbers_from_file('317.txt'),
    '502': load_numbers_from_file('502.txt'),
    '765': load_numbers_from_file('765.txt')
    # Add other area codes and their file paths here
}

# Check each number in the Excel file
for index, row in df.iterrows():
    raw_number = str(row['Phone'])  # Assuming column F contains phone numbers as strings
    cleaned_number = clean_phone_number(raw_number)

    # Skip invalid numbers and log them
    if not is_valid_number(cleaned_number):
        print(f"Skipping invalid number: {raw_number}")
        continue

    area_code = cleaned_number[:3]  # Extract the area code
    full_number = f"{area_code},{cleaned_number[3:]}"  # Reformat the number to match the text file pattern

    if area_code in numbers_by_area_code and full_number in numbers_by_area_code[area_code]:
        df.at[index, 'DoNotCall'] = 'DO NOT CALL'
        print(f"Lookup complete for {raw_number}: DO NOT CALL")
    else:
        df.at[index, 'DoNotCall'] = 'Ok to call'
        print(f"Lookup complete for {raw_number}: Ok to call")

# Save the updated Excel file
df.to_excel('updated_' + excel_file, index=False)
print("Excel file has been updated and saved as 'updated_" + excel_file + "'")
