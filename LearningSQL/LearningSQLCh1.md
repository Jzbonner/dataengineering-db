# Learning SQL 2nd Edition 
> If you are going to work with a relational database, whether you are writing applications, performing administrative tasks, or generating reports, you will need to know how to interact with the data in your database.

(_Learning SQL_, Alan Beaulieu)

## Introduction to Databases ~ Chapter 1
A __database___ is nothing more than a set of related information. A _database system_ is a computerized data storage and retrieval mechanism. A database system is able retrieve data more quickly, index data in multiple ways, and deliver up-to-the minute information to its user community. 

### Database Terminology 
Term | Definition 
--- | --- 
Entity | Something of interest to the database user community
Column | An individual piece of data stored in a table 
Row | A set of columns that together completely describe an entity or some action on an entity
Table | A set of rows, held either in memory or on permanent storage
Result Key | The result of an SQL Query 
Primary Key | One or more columns that can be used as a unique identifier for each row in a table 
Foreign Key | One or more columns that can be use together to identify a single row in another table

## What is SQL? 
SQL goes hand in hand with the relational model because the result of an SQL query is a table. Thus a new permanent table can be created in a relational database by storing the results set of a query. 

## SQL Statement Classes 
The SQL Language is divided into several distinct parts, of which include: _SQL schema statements_, which are used to define the data structures stored in the database; _SQL data statements_, which are used to manipulate the data structures previously defined by the schema; and _SQL transaction statements_, which are used to begin, end and roll back transactions. Below is an example od an SQL schema statement that creates a table called corporations, inserts data and accesses it: 

```SQL 
/* Example of an SQL Schema */
CREATE TABLE corporation: 
    (corp_id SMALLINT,
    name VARCHAR(30),
    CONSTRAINT pk_corporation PRIMARY KEY (corp_id)
    ); 

/* Example of an SQL data statement */
INSERT INTO corporation (corp_id, name)
VALUES (27, 'Acme Paper Corporation');

/* The following select statement would be used to actually retrieve the data fom the table created above */

SELECT name 
    FROM corporation 
    WHERE corp_id = 27; 
```

All database elements created via SQL schema statements are stored in a special set of tables called the data dictionary. This "data about the database" is known collectively as _metadata_. 

## SQL: A Nonprocedural Language
A procedural language defines both the desired results and the mechanism, or process, by which the results are generated. Nonprocedural languages also define the desired results, but the process by which the results are generated is left to an external agent. 

SQL statements define the necessary inputs and outputs, but the manner in which a statement is executed is left to a component of your database engine known as the _optimizer_. A general rule of thumb is that most if most SQL queries that you encounter or create will include some or all of the following SQL clauses; a general breakdown of their intended use can be seen below: 

```SQL 
SELECT  /* ONE OR MORE THINGS */
    FROM /* ONE OR MORE PLACES */
    WHERE /* ONE OR MORE CONDITIONS APPLY */
``` 
Along with querying your database, you will most liekely be involved with populating and modifying the data in your database. Meaning you should be making use of the `INSERT INTO` clause and the `VALUES` clause. 

If you are using an interactive tool such as the mysql command-line tool, then you will receive feedback concerning how many rows were either: 
* Returned by your `SELECT` statement 
* Created by your `INSERT` statement
* Modified by your `UPDATE` statement 
* Removed by your `DELETE` statement
