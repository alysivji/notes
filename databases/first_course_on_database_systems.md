# A First Course on Database Systems

By Jeffrey D. Ullman and Jennifer Widom

## Chapter 1: The World of Database Systems

* A Database Management System (DBMS) is a powerful tool for creating and managing large amounts of data efficiently and allowing it to persist over long periods of time, safely

DBMS is expected to:
  1. Allow users to create new databases and specify their schemas using a data-definition language
  2. Give users ability to query and modify data using a data manipulation (or query) language
  3. Support storage of large amounts of data over a long period of time
  4. Enable durability -- recovery of the database in the face of errors and failures
  5. Control access to data from many users without allowing unexpected interactions amongst users and without actions on data to be performed partially

* Query is parsed and optimized by a **query compiler**. The resulting query plan, or sequence of actions the DBMS will perform to answer the query, is passed to the execution engine.

* **transactions** are units that must be executed atomically and in isolation from one another

### ACID Property of Transactions

* **A**tomicity - all or nothing execution of transaction
* **C**onsistency - transactions are expected to preserve consistency of database
* **I**solation - transaction must appear to execute as if no other transaction is executing at the same time
* **D**urability - effect of transaction on database must never be lost once the transaction is completed

## Chapter 2: The Relational Model of Data

