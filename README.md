# Getting Started
This Repository contain details about connecting MySQL through Python and query data using Python. 
Attached is the .html file of Python code for reference. 

# Prerequisites
Step 1:
*To connect with MySQL Database, we have to first install its package in Python named "Pymysql", which will provide us the functionality for interacting with MySQL Databases*

#### Code
pip install pymysql

Step 2 :
*To create a connection with MYSQL Database, we need to use function called "create_engine", which is present in library called sqlalchemy*

#### Code
import pandas as pd
from sqlalchemy import create_engine
import pymysql
from tabulate import tabulate

##### parameter format of create_engine('SQL Dialect + Driver://username:password@hostname:port/database_name)
engine = create_engine('mysql+pymysql://username:password@localhost:3306/data_tools_assignment')

conn = engine.connect()

# Running the Test

Code:
result = conn.execute("SHOW DATABASES")
databases = [row[0] for row in result]
for database in databases:
    print(database)


Step 3 :
*Storing the result of the Query in a variable.*

Code:
df = pd.read_sql("SELECT * FROM data_tools_assignment.vgsales", conn)

### Breakdown of the Test
*one of the error, you can face while making a connection with the database is when your credentials (username and password) are not correct / working and you may encounter error message like below :
OperationalError: (pymysql.err.OperationalError) (1045, "Access denied for user 'root'@'localhost' (using password: YES)")*

*instead of reinstalling the whole 'MYSQL' set up again, shortest way is to By uninstalling MySQL Server 8.0 from MySQL Installer and reinstalling it, and set the credentials for MySQL again. Then try to connect with python with the credentials used in MySQL connection.*



## Attempting some Questions related to dataset

Q1.Was the average of global sales higher before or after 2005 ?

Code:

pd.read_sql('''(select 
(select round(avg(Global_Sales),2) 
from vgsales
where ((Year < '2005') AND (Year <> '1900'))
) as Average_Global_Sales_Pre_2005, 
(
select round(avg(Global_Sales),2) 
from vgsales
where Year >='2005'
) as Average_Global_Sales_Post_2005
)''',conn)

Result : Clearly Visible that Average Global Sales before 2005 is greater than Average Global Sales after 2005


Q2 : Create a new column that labels records before 2005 as 'pre-2005' and after 2005 as 'post-2005' ?

Code:
pd.read_sql('''(select V.Rank, V.Name,V.Platform,V.Genre,V.Publisher,V.Global_Sales,V.Year,
Case
when Year<'2005' then "Pre-2005" 
when Year>='2005' then "Post-2005" END as Label
from vgsales V)''',conn)

Result : Column with the name "Label" created in the end categorizing records into Pre-2005 and Post-2005



# Deployment
For attempting this project, make sure to have below software installed, ready, up and running:
MySQL : Make sure to have your credential setup (it will connect with python without password as well but above code is with password) & make sure your connection is up and running.
Jupyter Notebook : It can be directly setup or through any 3rd party distribution like Anaconda, Miniconda etc. Make sure to have the correct packages installed to connect with any relational database(MySQL in this case).

# Author
+ Shivam Dhawan

# License 
This project is licensed under the MIT License - see the LICENSE.md file for details

# Acknowledgements

This is one of the assignments provided by Professor Omar Al-Trad of Durham College for the course Data Analytics for Decision Making.

*Thank you Professor for providing the opportunity to work on this assignment !!*

