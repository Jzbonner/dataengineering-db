## RESTful API with Node.js and PostgreSQL 
A RESTful API is an API that coforms to the REST architectural style and constraints. REST systems are stateless, scalabale, cacheable and have a uniform interface. 

RESTful APIs most commonly utilize HTTP requests. Four the most common HTTP methods are GET, POST, PUT, and DELETE, which are methods by which a developer can create a CRUD system (create, read, update, delete)

### PostgreSQL
> PostgreSQL, commonly referred to as Postgres, is a free and open source relational database management system. You might be familiar with a few other similar database systems, such as MySQL, Microsoft SQL Server, or MariaDB, which compete with PostgreSQL.

**-blog.logrocket.com** 

You will have the option of utilizing a GUI version of PostgreSQL or the Command Line version; *please ensure you are using postgresql 11+.*

```sql 
/*
Database Initialization and Service Starter on Linux: 
$ sudo su postgres -l # or sudo -u postgres -i
$ initdb --locale $LANG -E UTF8 -D '/var/lib/postgres/data/'
$ exit

$ sudo systemctl enable --now postgresql.service

Connect to PSQL shell on Linux: 
$ sudo su postgres -c psql

*/

postgres=# \conninfo 

CREATE ROLE {user.name} WITH LOGIN PASSWORD {user.password}; 

ALTER ROLE {user.name} CREATEDB; 
```

### Express Server 
