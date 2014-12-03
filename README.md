MySQL to Redshift Converter
=============================

Fork of Lanyrd's MySQL to PostgreSQL conversion script. Use with care.

This script was designed for our specific database and column requirements -
notably, it quadrubles the lengths of VARCHARs due to a unicode size problem we
had, places indexes on all foreign keys.

This is way slower then coping the data via csv files to s3.

How to use
----------

Get the script:

    wget https://raw.githubusercontent.com/digi604/mysql-redshift-converter/master/db_converter.py

First, dump your MySQL database in PostgreSQL-compatible format

    mysqldump -h hostname -u username --password=MyPassword --port 3306 --single-transaction --routines \
    --triggers --databases my_database --compress  --compact --compatible=postgresql \
    --default-character-set=utf8 > my_database.mysql


Then, convert it using the dbconverter.py script

`python dbconverter.py my_database.mysql my_database.psql`

It'll print progress to the terminal.

Finally, load your new dump into a fresh PostgreSQL database using: 

`psql -h hostname -d databasename -U username -f my_database.psql -p 5439`
