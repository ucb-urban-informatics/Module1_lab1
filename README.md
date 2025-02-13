
# **Module 1 Lab -  Exploring and Analyzing Missing Files in a Project**


## **Objective**
You are working on a new project, where you need to organize a number of report files in `.docx` format. Each `.docx` file should have a `.pdf` version (with the same name) as well as a supplemntary `.csv` file but some `.csv` and `.pdf` files are missing. Your task is to identify the missing files and create a report of the name and count of missing `.pdf` and `.csv` files. 

## **Dataset Description**
A directory named **`report_files/`** contains:
- **a number of `.docx` files** (`report_<number>.docx`) → These are always present.
- **Some `.pdf` files are missing** (`report_<number>.pdf`).
- **Some `.csv` files are missing** (`Supplementary_data_R<number>.csv`).


### Your goal is to:
1. Identify missing `.pdf` and `.csv` files for each `.docx` file.
2. Count how many `.docx` files have missing `.pdf` and/or `.csv` files.
3. Create a folder to store the CSV report.
4. Generate two report files in `.csv` format, `missing_files_count.csv` and  `missing_files_list.csv` where you record the count and names of the missing files.

---

## **Tasks**
Create a Python file named `analyze_files.py` and save it. You will add the remaining code to this file.

### **1. Load and Explore the Project Files**


### **Step 1: List All Files in the `report_files/` Directory**
- Import the `os` library and use the `os.listdir()` function to retrieve a list of files inside the specified directory.
- Call `os.listdir()` and pass the directory path as an argument to get the list of files. (pass an argument means add it inside the parenthesis)

Example:

```python
import os

# List all files in the directory
files = os.listdir("report_files/")
print(files[:10])  # Display the first 10 files
```

---

### **Step 2: Process Files Using Loops, Conditionals, and String Methods**
Now that you have a list of files, you can:
- Use loops to iterate over the list of files.
- Apply conditionals to filter files based on their extensions.
- Use string methods to find files with specific patterns or extract cetrain parts of filenames.

---

### **Step 3: Categorize Files by Type**
- Create separate lists for each file type.
- Count the number of `.pdf`, `.csv`, and `.docx` files.



```python
import os

# List all files in the directory
files = os.listdir("report_files/")

# Create empty lists for file types
docx_files = []

# Loop through the list of files and check if it is .docx, .pdf, or .csv
for file in files:
    if file.endswith(".docx"):
        docx_files.append(file)
 
# Following the same logic used for `.docx` files, write code to create separate lists for `.pdf` and `.csv` files.

pdf_files = []



csv_files = []
    

# Print the counts
# Write your code here
```

### **Step 4:  Patterns Recognition (Computational Thinking)**
What was the block of code that you used more than once in the previous step?

Notice the recurring steps:
- Iterate over a list of files.
- Check if each file has a specific extension.
- Add the files files with specific extension to a list.

If you have 10 different file extensions to count, how many times do you need to repeat that block of code?

Rather than writing separate code for each file type, define a function that takes the list of all files and 
the file extension and returns the list of files with that extension. This approach makes the process reusable.

##### Note that we could also create these lists using an `if/elif` pattern, but to practice writing functions and improve modularity, we will define a function instead.

---
```python
import os

def get_files_by_extension(files_lst: list[str], extension: str):
    """Returns a list of files that match the given extension."""
    f_ext_list = []
    for file in files_lst:
        if file.endswith(extension):
            f_ext_list.append(file)
    return f_ext_list

# List of all files in the directory
files = os.listdir("report_files/")

# Get the list of files by extension
docx_files = get_files_by_extension(files, ".docx")
# pdf_files =  #add your code here then uncomment the line  
# csv_files =  #add your code here then uncomment the line

# Print the counts
print("Number of .docx files:", len(docx_files))
# do this for the other extensions you should work with
```

Using functions helps avoid repetition and makes the code easier to modify if new file types need to be checked.

## **2. Identify Missing `.pdf` and `.csv` Files**  
Now that we have lists of `.docx`, `.pdf`, and `.csv` files, let's check which reports are missing their required counterparts.

### **Step 1: Breaking Down the Problem:**
- Every `report_<number>.docx` should have:
  - A corresponding `report_<number>.pdf`.
  - A corresponding `Supplementary_data_R<number>.csv`.

### **Step 2: Count and Report Missing Files**
1. Extract report numbers from the `.docx` filenames.
   - The filenames follow this pattern: `report_<number>.docx`.
   - Use `.split("_")` to separate `report` from the number. (If you don’t understand this step, try printing  
     `file.split("_")` inside a script to see how it works.)
   - Take the second part of the split result which is `<number>.docx`.
   - We want to remove the `.docx` part of the string by replacing it with an empty string `""`, effectively removing it.
   - Now we have the `<number>` extracted.

2. Use fstring to add `<number>` and `.pdf` to the "report_" and Check if `report_<number>.pdf` exists.
3. Use the same logic used in previous step, check if `Supplementary_data_R<number>.csv` exists.
4. Count and report the number of `.docx` files that are missing `.pdf` and/or `.csv` files.

---

```python


docx_report_numbers = [] 

# Extract report numbers from the `.docx` filenames.
for file in docx_files:
    # Use `.split("_")` to separate `report` from the number. 
    parts = file.split("_")  
    # Check to make sure we are splitting the correct file name pattern
    if len(parts) == 2 and parts[0] == "report":  
        # Take the second part of the split result which is `<number>.docx`.
        # Replace .docx with an empty string `""` to remove it
        report_number = parts[1].replace(".docx", "") 
        # add the number to our list 
        docx_report_numbers.append(report_number)
    else:
        print(f'{file} does not match the pattern!')
        
# Count missing pdfs
num_missing_pdfs = 0  
missing_pdfs = []
for number in docx_report_numbers:  
    pdf_name = f"report_{number}.pdf"  
    if pdf_name not in pdf_files:  
        missing_pdfs.append(pdf_name)
        num_missing_pdfs =  num_missing_pdfs + 1  

print("Number of missing .pdf files:", num_missing_pdfs)


# Count missing csvs:
# write the code for counting and recording the missing csvs
# Hint: use the csv name pattern here 

```


---

## **3.Creating a Folder to Store Reports**  

Now that we have identified the missing `.pdf` and `.csv` files, we need to save the results in a structured report. 
Instead of saving the report directly in the main directory, we will **create a new folder** to store the report.

#### **Why Create a Folder?**
- It keeps the reports **organized** and separate from the dataset files.
- It makes it easier to **manage multiple reports** over time.
- It avoids clutter in the **main directory**.

#### **How to Create a Folder in Python**
In Python, you can create a new folder using the `os.makedirs()` function. 
This function ensures that the folder exists before attempting to write files into it. 
If the folder already exists, it does **not** create a duplicate or raise an error.

#### **Example:**
```python
import os

# Define the folder name
report_folder = "missing_reports"

# Create the folder if it does not exist
os.makedirs(report_folder, exist_ok=True)
```

#### **How `os.makedirs()` Works:**
- If `missing_reports/` does **not** exist, it **creates** the folder.
- If `missing_reports/` folder **already exists**, the function does **nothing** because of the `exist_ok=True` argument. 
- Unlike `os.mkdir()`, which only creates a single-level directory, `os.makedirs()` can also create **nested directories** if needed.


This ensures that when we later save the CSV report, the folder is available and ready.

---



## **4. Write Missing File Reports to CSVs**  
The missing file data will be **saved to two CSV files** for documentation and further analysis.

### **Num Missing Files Report (`missing_files_count.csv`)**  
This report contains the total count of missing files by type.

```python
import csv

# Previously created this folder
report_folder = "missing_reports"

# Define the file path
missing_count_csv_path = report_folder + "/" + "missing_files_count.csv"

# Write missing file counts
with open(missing_count_csv_path, mode="w", newline="") as csvfile:
    writer = csv.writer(csvfile)
    
    # Write header
    writer.writerow(["File Type", "Count"])

    # Write total counts
    writer.writerow(["PDF", num_missing_pdfs])
    writer.writerow(["CSV", num_missing_csvs])

print("Missing file count report saved to:", missing_count_csv_path)
```

---

### **Name of Missing Files Report (`missing_files_list.csv`)**  
This report contains the names of each missing `.pdf` and `.csv` file.

```python
import csv

# Previously created this folder
report_folder = "missing_reports"

# Define the file path
missing_names_csv_path = report_folder + "/" + "missing_files_list.csv"

# Write missing file names
with open(missing_names_csv_path, mode="w", newline="") as csvfile:
    #define our writer, this will be used to write rows into our csv file
    writer = csv.writer(csvfile)
    
    # Write header
    writer.writerow(["File Type", "Missing File Name"])

    # Write missing PDFs
    for pdf in missing_pdfs:
        writer.writerow(["PDF", pdf])
    
    # Write missing CSVs
    for csv in missing_csvs:
        writer.writerow(["CSV", csv])

print("Missing file list saved to:", missing_names_csv_path)
```

---

## **Deliverables**
1. **Python Script (`analyze_files.py`)**  
   - Prints:
     - Total number of `.docx`, `.pdf`, and `.csv` files.
     - Number of `.docx` files missing `.pdf` and/or `.csv` files.
   - Saves missing file reports in the `missing_reports/` folder:
     - **`missing_files_count.csv`** – Number of missing files per type.
     - **`missing_files_list.csv`** – Names of missing `.pdf` and `.csv` files.

---


## **Hints**
- Use **sets** for efficient file comparison.
- Write modular functions for reusability. 

Good luck!
