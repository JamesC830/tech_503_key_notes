# Database notes

***Database***: Structured collection of data

## Relational databases

Databases are related to eachother with common columns between them

**Primary key:**

- Unique identifier for each record
- Cannot be changable
- ***Composite***: If two columns are required to make a unique ID

**Foreign keys:** Reference primary keys of other tables

**Data redundancy:** Repeating information in a database

**Data model types**
![data model types](https://vertabelo.com/blog/types-data-models/2.png)

### Normalisation

- ***Pros***: Reduces redundancy, ease of maintenance
- ***Cons***: Increases depth, more difficult to use


**Normalisation forms:**

- ***1NF***: Data must be atomic (As small as possible), Unique rows
- ***2NF***: + Functionally dependant (non-key attributes must depend upon the full primary key)
- ***3NF***: + No transitive dependancies (i.e. no A depends on B which depends on C)
