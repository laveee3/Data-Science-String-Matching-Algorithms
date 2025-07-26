# 🔍 Data Science: String Matching Algorithms for Data Synchronization

This project focuses on **string matching and data cleaning techniques** to synchronize contact information (email, phone, and address) between large CSV files and an Oracle database containing millions of records.  

The goal is to **identify outdated contact information in the database** and update it using the most recent details from CSV files.  

---

## 📌 Problem Statement

- The Oracle database contains old and incomplete data of individuals associated with various institutions.  
- CSV files (ranging from **1,000 to 500,000 records**) contain updated contact information.  
- For each record (identified by a unique `EID`), the CSV contact details must be **compared against the database**.  
- Any new phone numbers, emails, or addresses missing from the database must be identified and updated.  

---

## ✅ Solution Approach

### **Tools & Libraries**
- **Python 3.6**, **Anaconda**, **Jupyter Notebook**
- Libraries: `pandas`, `numpy`, `matplotlib`, `fuzzywuzzy`, `cx_Oracle`

### **Step-by-Step Workflow**
1. **Load and preprocess CSV data**  
   - Read into pandas DataFrame, retain relevant columns, remove duplicates & empty `EID` rows.  
   - Drop rows missing all contact information.  

2. **Clean and format contact information**  
   - Standardize phone numbers using regex (remove special characters, unify area codes).  
   - Clean email addresses and addresses.  

3. **Fetch database data**  
   - Retrieve database records using batched `EID` queries (1000 IDs per query).  
   - Format database contact info to match CSV formatting.  

4. **Compare CSV and database records**  
   - **Emails:** Exact string match  
   - **Phones:** Unified 10-digit comparison  
   - **Addresses:**  
     - Used **Levenshtein Distance** with `fuzzywuzzy` (Token Set Ratio > 83)  
     - Handles variations in zip codes, city/state differences  

5. **Identify and record new contact information**  
   - Emails, phones, and addresses not present in the database are collected into separate DataFrames.  
   - DataFrames are merged (outer join) to create a unified update file.  

6. **Visualize differences**  
   - Venn diagrams and bar plots created using `matplotlib` to show how many records need updates.  

7. **Output**  
   - New contact information is exported into **organized Excel files** for easy data entry.  
   - Records are programmatically sorted by type of update (Address, Phone, Email).  

---

## 🧪 String Matching Algorithms Used

- **Exact string comparison:** Emails  
- **Regex-based formatting:** Phone numbers  
- **Levenshtein Distance (Fuzzy Matching):** Physical addresses  
  - Implemented with `fuzzywuzzy.token_set_ratio`  
  - Handles variations in abbreviations, partial matches, and typos  

---

## 📊 Sample Visualizations

- Distribution of updated addresses, phones, and emails  
- Venn diagrams showing overlap between database and CSV data  

*(Visuals generated using matplotlib, included in the notebooks)*  

---

## 📂 Repository Structure
Data-Science-String-Matching-Algorithms/
│
├── input/
│ ├── FakeContact.csv # Sample input CSV
│ └── db_data.xlsx # Sample database data (Excel)
│
├── notebooks/
│ └── DS_fuzzyLogic_Visualization.ipynb # Main notebook
│
├── scripts/
│ └── split_csv.py # Utility for splitting large CSVs
│
├── output/
│ └── Updated_Contacts.xlsx
│
├── FakeContact_Generator.py # Generates fake CSV data
└── README.md


