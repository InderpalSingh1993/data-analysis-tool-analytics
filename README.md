# Data Analytics Tool Analytics

- Step 1 - Importing Required Python Libraries
```python
import pandas as pd
from sqlalchemy import create_engine
import pymysql
```
- Step 2 - Create Database Engine
```python
# Replace username and hostname with your database username and host
engine = create_engine('mysql+pymysql://username:@hostname')
```
- Step 3 - Connect a database engine 
```python
conn = engine.connect()
```
- Step 4 - Create a new column that labels records before 2005 as 'pre-2005' and after 2005 as 'post-2005'
```python
# If year is greater than 2005, then it will show "post 2005" else "pre 2005"
complex_df = pd.read_sql('''SELECT *, case
WHEN Year > 2005 THEN "post 2005"
WHEN Year < 2005 THEN "pre 2005"
ELSE null
END AS Released
from data1202.videogamesales''', conn)

complex_df.head(20)
```

- Step 5 - Query to print sales before 2005 and after 2005 and then comparing it
```python
#Was the average of global sales higher before or after 2005 ?
complex_df = pd.read_sql('''Select avg(Global_Sales) as Global_Sales, Released from (SELECT *, case
WHEN Year > 2005 THEN "post 2005"
WHEN Year < 2005 THEN "pre 2005"
ELSE null
END AS Released
from data1202.videogamesales) as a
Group by Released''', conn) 

complex_df.head()
```
- Step 6 - Another way to add a Released column showing pre-2005 if year is release is before 2005, display post-2005 if year of release is after 2005, otherwise display null
```python
complex_df.loc[(complex_df['Year']>2005), 'Released'] = "Post 2005"
complex_df.loc[(complex_df['Year']<2005), 'Released'] = "Pre 2005"
complex_df.loc[(complex_df['Year']==2005), 'Released'] =  "null"

complex_df.head(20)
```