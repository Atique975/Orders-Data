To

#!/usr/bin/env python
# coding: utf-8

#import libraries 
!pip install kaggle 
import kaggle

!kaggle datasets download ankitbansal06/retail-orders -f orders.csv

#extract file from zip file
import zipfile 
zip_ref = zipfile.ZipFile ('orders.csv.zip')
zip_ref.extractall()
zip_ref.close()

#read data from the file and handle null values
import pandas as pd
df = pd.read_csv('orders.csv', na_values=['Not Available','unknown'])

df['Ship Mode'].unique()

#rename columns names ..make them lower case and replace space with underscore
#df.rename(columns={'Order Id':'order_id', 'City':'city'})
#df.columns=df.columns.str.lower()
#df.columns=df.columns.str.replace(' ','_')
df.head(5)

df.columns= df.columns.str.lower()
df.columns=df.columns.str.replace(' ', '_')

#derive new columns discount , sale price and profit
df['discount'] = df['list_price']* df['discount_percent']/100
df['sale_price'] = df['list_price'] - df['discount']
df['sale_price'] = df['list_price'] - df['discount']

#convert order date from object data type to datetime
df['order_date']=pd.to_datetime(df['order_date'], format = '%Y-%m-%d')

#drop cost price list price and discount percent columns
df.drop(columns=['list_price','cost_price','discount_percent'],inplace=True)


#load the data into sql server using replace option
pip install sqlalchemy pymysql
import sqlalchemy as sal
from sqlalchemy import create_engine
username = '****'
password = '********'  
host = 'localhost'
database = 'sys'

connection_string = f'mysql+pymysql://{username}:{password}@{host}/{database}'
engine = create_engine(connection_string)
conn = engine.connect()
result = conn.execute("SELECT DATABASE();")
database_name = result.fetchone()[0]
print(f"The current database is: {database_name}")


df.columns
df.drop(columns=['order_dare'], inplace=True)
df.to_sql('df_orders', con=conn, index= False, if_exists ='append')

