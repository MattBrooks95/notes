# Chapter 11 Safe Database Access

## 3 types of database abstractions in Haskell
1. types that store haskell data structures on disk in a binary format, like acid-state
    - has bad interoperability w/other systems
    - ties your application data to Haskell (if you used a db proper, you could interact with the db from another programming language if you needed to)
2. libraries that wrap a particular database, like sqlite-simple, mysql-simple, pgsql-simple (for postgres), MongoDB
3. libraries that wrap over many kinds of databases
    - even though they can technically abstract over many databases, the API might be a subset of what your DB can actually do
    - you will typically import some functions that can technically only work for a particular database
    - there is a library called HaskellDB that abstracts over a relational database, but you need to write your queries with a language that is not standard sql (relational algebra)
    - Persistent is like an ORM for Haskell, but it cares about things like immutability and and referential transparency (you know when you are calling code that is going to hit the database)

## Persistent and Esqueleto
Persistent can model relational and non-relational databases, but it is biased towards relational ones in terms of the features that it supports

Esqueleto is a library that provides functionality that Persistent doesn't and is like a companion library. It allows for more expressive queries that use JOINs. Using Esqueleto means that you will become unable to use some of Persistents nosql features.

### Connections
- persistent needs to be paired with a backend built for a particular database
    - persistent-sqlite
    - persistent-postgresql
- the `with<DBMS here>Conn` function is used for getting a single connection to a database
    - `withPostgresqlConn` is an example of a concrete function name for `with<DBMS here>Conn`
    - the connection is then passed into `runSqlPersistM`
- `with<DBMS here>Pool` and `runSqlPersistMPool` functions can be used to get access to pooled connections, which is a performance win and a must for production systems of size
    - `withPostgresqlPool` is an example of a concrete function name for this

### Schema and Migrations
- persistent needs to know the database schema and the migrations
    - `persistent-template` is a library that will use Template Haskell to generate code for you
- `persistent-template` will generate glue code and types for you to use to model your database in haskell
    - write definitions of objects in a DSL that is written inside of quasiquotes
    - if you write `json` next to the name of an entity, persistent-template will write in the ToJSON and FromJSON instances of your data types for you
- `persistLowerCase` QQ will turn entity names into snake_case when it creates database column names
    - for example, `myType String` would become a column `my_type` in the database
    - you can also have it derive instances like Show for you in this way
- `persistent-template`  will implicitly create IDs for each object type in your schema
    - if a field in an object is written with the first letter Uppercased, then that field will become a uniqueness constraint on the column? in the database
    - adding `Maybe` after a column name in the object definition means that that column will become Nullable in the database
- enums
    - define the enum type in Haskell
    ```
    data PetType = Dog | Cat deriving (Show, Read, Eq)
    ```
    - in your schema definition, you can use your enum type like this
    `derivePersistField "PetType"` <- this is a top-level function call in a Haskell module, template haskell will associate this string name with the name of your actual enum type
    - because of some limitations of Template Haskell, the enum definitions must be written in a different file than the file where you tell `persistent-template` to generate the types for you
- migrations
    - in order to make changes to the DB schema (adding tables, removing columns), you need to tell Persistent to make a migration
    - Persistent can't create a migration without knowing the schema
    - you can use the `share` function to share the schema between the database builder functions and the migration functions
    - persistent will log the SQL that was ran, for debugging purposes
    - two steps for a migration
        1. make the migration
        2. run the migration: `runMigration migrateAll` where `migrateAll` is the name that you specified when you called `mkMigrate`
    - when you add a column to an entity, you must provide a default value that will be used to fill in that field in any data that already existed in the db
- when you need to work with a database that has already been built, you can tell persistent what table and columns should be used to line up with the existing database schema
- you cannot use Integer or Int in Persistent, you must use Int64


