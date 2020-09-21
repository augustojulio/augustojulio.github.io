---
layout: post
title: How to read a CSV file quickly using Python, Pandas and save into a PostgreSQL database with SQLAlchemy ORM
---

Article wrote in august/2020 and published in my Medium account(https://medium.com/@augustojulio/)

Some weeks ago I had searched how to read from a CSV file and save in a database using Python, Pandas and as I didn’t find much material, I’ve decided to write this article.

I chose Pandas to read the CSV because it’s a powerfull library to manipulate data and I chose SQLAlchemy because it’s a popular Python ORM.

Create a python file, declare the libs, read DB credentials from .env file and create the DB connection.

'''
import pandas as pd
import psycopg2
import os
from dotenv import load_dotenv
from sqlalchemy import create_engine
from sqlalchemy.types import Integer, TIMESTAMP, FLOAT, VARCHAR
project_folder = os.path.dirname(
os.path.abspath(“project_get_family_info”)
)
load_dotenv(os.path.join(project_folder, ‘.env’))
db_uri_fmt = (
 ‘{dialect}+{driver}://{user}:{password}@{host}:{port}/{database}’
)
db_uri = db_uri_fmt.format(
 dialect=’postgresql’,
 driver=’psycopg2',
 user=os.environ[‘DB_POSTGRES_USER’],
 password=os.environ[‘DB_POSTGRES_PASSWORD’],
 database=os.environ[‘DB_DATABASE’],
 host=os.environ[‘DB_HOST’],
 port=os.environ[‘DB_PORT’],
)
engine = create_engine(db_uri)
'''

Now, it’s time to read the csv file and save the info in your database.

'''
content = pd.read_csv(‘data/csv_data.csv’)
table_name = ‘family’
content.to_sql(
 table_name,
 engine,
 if_exists=’replace’,
 index=False,
 dtype={
 “id”: Integer,
 “create_date”: TIMESTAMP,
 “name”: VARCHAR,
 “age”: FLOAT,
 “relationship”: VARCHAR,
 }
)
'''

That’s it!
