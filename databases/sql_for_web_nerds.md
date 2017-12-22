# [SQL for Web Nerds](http://philip.greenspun.com/sql/index.html)

by Philip Greenspun

## [Chapter 1: Introduction](http://philip.greenspun.com/sql/introduction.html)

### ACID Test

Transaction processing requires:

* __A__tomicity - transaction is either committed or rolled back. All changes take efffect, or none do.

* __C__onsistency - database is always transformed from valid state to valid state. Transaction is legal only if it meets all relation integrity contstraints; if not legal it is rolled back

* __I__solation - each transaction is invisible to other transactions until it is complete

* __D__urability - once a transaction is complete, the results are permanent

> From an application programmer's point of view, the biggest innovation in the relational database is that one uses a declarative query language, SQL (an acronym for Structured Query Language and pronounced "ess-cue-el" or "sequel"). Most computer languages are procedural. The programmer tells the computer what to do, step by step, specifying a procedure. In SQL, the programmer says "I want data that meet the following criteria" and the RDBMS query planner figures out how to get it. There are two advantages to using a declarative language. The first is that the queries no longer depend on the data representation. The RDBMS is free to store data however it wants. The second is increased software reliability. It is much harder to have "a little bug" in an SQL query than in a procedural program. Generally it either describes the data that you want and works all the time or it completely fails in an obvious way.

### Declarative Query Language

> In SQL, the programmer says "I want data that meet the following criteria" and the RDBMS query planner figures out how to get it. There are two advantages to using a declarative language. The first is that the queries no longer depend on the data representation. The RDBMS is free to store data however it wants. The second is increased software reliability. It is much harder to have "a little bug" in an SQL query than in a procedural program. Generally it either describes the data that you want and works all the time or it completely fails in an obvious way.

### Database Basics

> A relational database is a big spreadsheet that several people can update simultaneously.
>
> Each table in the database is one spreadsheet. You tell the RDBMS how many columns each row has. For example, in our mailing list database, the table has two columns: name and email. Each entry in the database consists of one row in this table. An RDBMS is more restrictive than a spreadsheet in that all the data in one column must be of the same type, e.g., integer, decimal, character string, or date. Another difference between a spreadsheet and an RDBMS is that the rows in an RDBMS are not ordered.

### Data Manipulation Language

* SQL

### Relational DB vs Object DB

> Object databases bring back some of the bad features of 1960s pre-relational database management systems. The programmer has to know a lot about the details of data storage. If you know the identities of the objects you're interested in, then the query is fast and simple. But it turns out that most database users don't care about object identities; they care about object attributes. Relational databases tend to be faster and better at coughing up aggregations based on attributes. The critical difference between RDBMS and ODBMS is the extent to which the programmer is constrained in interacting with the data. With an RDBMS the application program--written in a procedural language such as C, COBOL, Fortran, Perl, or Tcl--can have all kinds of catastrophic bugs. However, these bugs generally won't affect the information in the database because all communication with the RDBMS is constrained through SQL statements. With an ODBMS, the application program is directly writing slots in objects stored in the database. A bug in the application program may translate directly into corruption of the database, one of an organization's most valuable assets.

---

## [Chapter 2: Data Modeling](http://philip.greenspun.com/sql/data-modeling.html)

* __data modeling__ is the process of defining tables; we are defining:

> * what elements of the data you will store
> * how large each element can be
> * what kind of information each element can contain
> * what elements may be left blank
> * which elements are constrained to a fixed range
> * whether and how various tables are to be linked

* When building the data model you must affirmatively decide whether a NULL value will be permitted for a column and, if so, what it means.
