Data Pipeline : Scraping, cleaning, converting, storing and Querying the data.

Overview:
This module 1 scrapes the data from books.toscrape.com, cleans/converts the data, load and store it in sql tables and pandas dataframe and then query the data as per the requirement.

For the setup, install and import the packages from requirements.txt and run the code available in Capstone_Module_1.py file.

In Capstone_Module_1.py, we are performing below actions:

1. Scraping the data:
Scraped first 5 pages data of all categories and captured title, price, rating, availability and category into a dataframe.

2. Cleaned the data:
a) price - stripped the characters and converted to float and stored as price_gbr
b) Rating - maped from text (One to five) to 1-5.
c) Availability - converted to Boolean and stored as in_stock
d) Error handling - median imputation for numeric fields and dropped row for object/string fields if any rows are null/blank.

3. for fixed baseline constant, created price_inr using below expression
	price_inr = price_gbp * 105.50

4. Created normalized SQL schema:
created two sql tables categories and books with primary and foreign key relationship, this normalized schema will avoid data duplication.

5. Querying data with SQL:
Ran sql queries to fetch the data using select, where, distinct, limit, order by and join.

6. Compared the outputs b/w SQL and pandas dataframe:
showing the results from pd.read_sql() and pd.merge() and comparing the results to see if it matches.



