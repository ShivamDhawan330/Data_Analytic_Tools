# Data_Analytic_Tools
This Repository contain details about connecting MySQL through Python and query data using Python

Step 1:
To connect with MySQL Database, we have to first install its package in Python named "Pymysql", which will provide us the functionality for interacting with MySQL Databases

Code
pip install pymysql

Step 2 :
To create a connection with MYSQL Database, we need to use function called "create_engine", which is present in library called sqlalchemy

Code
import pandas as pd
from sqlalchemy import create_engine
import pymysql
from tabulate import tabulate

engine = create_engine('mysql+pymysql://username:password@localhost:3306/data_tools_assignment')

conn = engine.connect()


Step 3 :
Storing the result of the Query in a variable.

Code:
df = pd.read_sql("SELECT * FROM data_tools_assignment.vgsales", conn)

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



