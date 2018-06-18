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

* Data model is a notation for describing data or information. Consists of three parts:
    * Structure of Data
    * Operations on Data
    * Constraints on Data

* Relational data is stored in tables (rows and columns), although the underlying implementation maybe something complex
* Semistructured data resembles trees or graphs, hierachically nested tags

* semistructured models have more flexibility than relations, but since databases are large, efficiency of access to data and efficiency of modifications are of great importance.
    * limitations become features, we optimize for the one use case
    * if our data is relational, half the work is already done for us

### Relation (aka Table)

* Attributes - named columns of a relation
* Schemas - relation name + set of attributes
* Tuples - rows of a relation
* Domain - data type
* Relation - sets of tuples

* We expect tuples to change, but the schema is less likely to change
    * schema changes ('migrations') are expensive
    * if we add an attribute, we have to generate appropriate values for new components in the existing tuples

* Key - set of attributes form a key if we do not allow two tuples in a relation instance to ahve the same values in all the attributes of the key
    * Databases use artifical keys to not make any assumptions about attribute values

### Defining a Relation Schema in SQL

* `CREATE TABLE`
* `ALTER TABLE` then `ADD` or `DROP [attribute_name]`

* Two tuples in R cannot agree on all of the attributes in set S, unless one of them is NULL. Any attempt to insert or update a tuple that violates this rule causes the DBMS to reject the action that caused the violation.

* if `PRIMARY_KEY` is used, then attributes in S are not allowed to have `NULL` as a value for their components

### Algebraic Query Language

> By limiting what we can say or do in our query language, we get two huge rewards - ease of programming and the ability of the compiler to produce highly optimized code

> If all we could do was to write single operations on one or two relations as queries, then relational algebra would not be nearly as useful as it is. However, relational algebra, like all algebras, allows us to form expressions of arbitrary complexity by applying operations to the result of other operations.

> One can construct expressions of relational algebra by applying operators to subexpressions, using parentheses when necessary to indicate grouping of operands. It is also possible to represent expressions as expression trees; the latter often are easier for us to read, although they are less convenient as a machine-readable notation.

### Constraints on Relations

> A common kind of constraint, called a referential integrity constraint, asserts that a value appearing in one context also appears in another, related context.
