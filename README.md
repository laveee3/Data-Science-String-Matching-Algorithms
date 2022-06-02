# Data Science
Problem:
--------
Compare and find the difference between huge data file(csv)(contains latest data) and database data (not up-to-date) and update the database with new info from csv file.
Oracle database - has millions of records of the people who belong and who belonged to the Institutions. 
Various details of the people are there. This work revolves around the contact information like email, phone and address.
Many a times we get large csv files with current details of people. The csv files would have records between 1000 to 500000.
This means each record in the csv file need to be verified against the database. And if any of the phone/email/address contact information between the csv file and database does not match, the info in the csv file needs to be entered into the database. 
For every record there is an unique identifier- ID
Database data are old and data from csv are new.

Solution:
---------
_Software used: Python 3.6, Anaconda, Jupiter Notes
Libraries used: numpy, panda, matplotlib, fuzzywuzzy, cx_Oracle_

The programs step by step procedure:
1.	The csv input data is passed to the panda dataframe and needed columns are retained, duplicate rows and empty Eid rows are removed.
2.	This csv dataframe is further narrowed down by few operations. Finally just the unique identifier and one contact data is retained. 
3.	Rows that does not have even a single contact information are removed.
4.	CSV dataframe contact data needs cleaning and formatting. Its being done.
5.	The EID of all the records alone are fed to a list variable as set of 1000s since that’s is the maximum limit of the query’s IN statement.
6.	Database connection is being established and using sql query a relevant table is accessed. The needed columns for all the EID in the csv file are extracted from database.
7.	Database data is being fed to another dataframe.
8.	Now we have the dataframe from csv file and dataframe from database for the same EID.
9.	For record in database, there maybe one or more phone numbers, email address, address (home or office or both). Database dataframe is also formatted to match with the format we reached in step 4.
10.	Each contact in csv file is compared against contact fetched from database. There may be more than one contact in the database. Csv_df is used in the for loop and csv_contact is compared with database_contact details. If the csv contact information is not present in database, then its being added to a dataframe that contains all new details. 
11.	For address comparison, Levenstein distance concept is being used. Csv/db Email comparison is a string comparison. For phones, regex are used in data formatting in one of input sources.
Csv Email address that is not present in the database is added to a email_dataframe. These emails will be updated in the database later.
12.	Data unification/cleaning for the phone numbers present in both database and csv is done. Phone numbers comes in different format, it could be continuous number or it could brackets around area code or ‘-‘between area codes and rest of the numbers. Each phone address in csv file is compared one or more phone address present in the database for the relevant EID.  Phones from the dataframe is split into two columns Area and Phone. So, we join Area+phone to get the 10 digit number in a format that matches the format we reached in csv file input data formatting. 
Some  ‘Area’ column are empty. When area+phone operation is done it gives number like this None1234567890. In this case, even a matching number is thrown as new number. Though it looked simple, to spot empty firld in Area but it took long time figure ot. 
Dataframe techniques to spot these rows using “startswith & replace” feature did not work and going line by line and using if statement for empty area field also did not work. Finally, the below piece of code solved the problem. It is finding the rows with empty area data, storing them in a separate dataframe and deleting these rows from db_df and after formatting empty_area_df then appending to db_df.
13.	Csv Phone address that is not present in the database is added to a phone_dataframe. These phones will be updated in the database later.
				For physical address comparison:
				_Logic 1:_
				if the number of lines matching EID is more than 1, create a df to put those many rows, "rows" while rows >0 and 
				 break the loop if the addr comparison for that loop yields token_set_ratio > 83
				If zipcode differs, state or city or house inside same city might be different
				If zipcode is same, compare address line 1 and line 2
				if zipcode is not given, compare city or state before address comparison

				_Logic2:	_
				Levenstein Distance helped very well. Just the street address line 1 is enough.
				Fuzzywuzzy tool is so helpful. Of the many tokens (like partial token, token sort ratio), token set ratio perfectly fits the need. This helped to find the matching address and keep adding to the addr_dataframe with records with new address that is not in the database. Logic 2 is very light, easy and fast than the logic 1.

14.	Matplotlib is used to show how many records have new address from the total csv records.
15.	All the dataframes that has new phone/email/address are merged in pandas dataframe like sql outer join merge.
16.	More visualization is also done by venn diagram

Output files programmatically sorted to make the data entry easy:
Data Visualization in Python
All three output files described above are merged and made into one big file. Again, another Python data science program was built to sort the big file into various ways, so that it is helpful for data entry people. Below is the way they are sorted and excel files are in the folder: 

Changes in:	Number of records
Address, Email, Phone	3XX
Address, Email	1XXX
Address, Phone	1XXX
Email, Phone	2XXX
Address	1XXXX
Email	6XXX
Phone	1XXXX
Total Changes	3XXXX

The code that I have uploaded is rewritten instead of uploading the original. In this program, i have used fake data as input and also its a small file. This have reduced many lines of code which was written to handle the scalability for big-data. Not only the lines of code, but also the number of programs used. What was originally 4 separate programs is made into one here. For real data and huge data we cant use this single file, it needs to be 4 separate py scripts. Also, I have included the code that connects to the data base but for time-being I am importing database data from excel file.

Please free to download this folder and see the results by running the program.

Files: Input files - FakeContact.csv and db_data
Program: DS_fuzzyLoic_Visualization

Other files:
These were used in the original work but not needed here. Still since they are related have included in this package.
Program: Fakecontact - program that generated fake data, 
split csv - to split csv


