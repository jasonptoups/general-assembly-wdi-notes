# Relational DBs and SQL

## What it is
* Relational databases are like Excel
  * table is a sheet
  * databse is a workbook
* Tables can relate to each other by joins or other methods. You typically need a key that matches.
* We have a one-to-many relationship
* We can have dynamic relationships between tables

## SQL
* S Query Language
* Why use it?
  * Most data starts out as relational and structured

## Using PostgreSQL
1. Open and run the PostGres in your top tab. Initialize.
2. In CLI:
```bash
$ psql
# This will launch the psql command line
$ help
$ \l 
# this lists the databases
$ CREATE DATABASE generalassembly;
# have to use the semicolon
$ \c generalassembly
# this connects to the generalassembly database
$ \d 
# this lists all the tables
$ CREATE TABLE students (
# opens the parens and you can enter data

generalassembly=# CREATE TABLE students (
generalassembly(#   id SERIAL PRIMARY KEY,
generalassembly(#   first_name VARCHAR NOT NULL,
generalassembly(#   last_name VARCHAR NOT NULL,
generalassembly(#   quote TEXT,
generalassembly(#   birthday VARCHAR,
generalassembly(#   ssn INT NOT NULL UNIQUE
generalassembly(# );

generalassembly=# INSERT INTO students (first_name, last_name, ssn) VALUES ('Jimmy', 'Buttons', 123401234);

\d 
# shows the tables

SELECT * FROM students;
# Shows the table students

UPDATE students SET first_name = 'Jim' WHERE first_name = 'Jimmy';
# updates Jimmy to Jim

DELETE FROM students WHERE first_name = 'Jim';
# Deletes

DROP TABLE students;
# deletes the table

DROP DATABASE generalassembly;
```
### On Schema:
Each table has a Schema. It has the following:  
1. Column name
2. Column's data type
3. Constraints  

Some key words:
* VARCHAR: text or numbers with limited length
* TEXT: string with unlimited length
* INT: integer
* SERIAL: a serial number
* BOOLEAN: boolean
* DATE: date
* TIME: time
* NOT NULL: required
* UNIQUE: has to be unique
* PRIMARY KEY: a random key that we use to identify each row

## SQL syntax rules
* lowercase variables and table names
* use _ insteead of -
* plural name for tables, singular for columns
* \ commands are for psql
* CAPS COMMANDS are for SQL

## One to Many
![One to Many](https://git.generalassemb.ly/ga-wdi-lessons/sql-intro/raw/master/images/one_to_many.png)
In this example, each author has many books while each book has 1 author. Each book as an author_id, and we are able to look up the author_id in authors
