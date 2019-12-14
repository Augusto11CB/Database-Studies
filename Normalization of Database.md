
# Normalization of Database

## To do
[Guru99 - Db Normalization](https://www.guru99.com/database-normalization.html)

## What Normalization is?
Normalization is the action/technique of organizing the data in a databese. It is a systematic approach to disarrange and rearrange tables in order to eliminate data redundancy(repetition) and anomalies while executing insertion, update and deletion.

## Problems Without Normalization
if the database is not correctly normalized and have redundancy there are problems of inefficient memory utilization and operations such as update will be difficult to be performed due to the higth dependency among columns and tables.

### Table to use as BAD example
| EmployeeID | SalesPerson |  SalesOffice | Number |Customer 1 | Customer 2 | Customer 3 |  
| -- | -- | -- | -- | -- | -- | -- | 
|1003  | Mary | Chicago | 1234-56789 | Ford | GM  |  |
| 1004 | Joo | NY | 98765-4321 | Dell | HP | Aple
| 1005 | Martin | Chicago  | 1234-56789  | Boing |  |


### Modification Anomalies
Taking in account the above table, let's suppose that the NY Sales office  move to Orlando, Florida. Thus, every row with salesoffice equals NY need to be updated. 
These situations are modification anomalies. There are three modifications anomalies.

#### Insert Anomaly
Sometimes is not possible to persiste information because the full row information is not available. 
Using the table above as example, the insertion of a new  **sales office** is not allow since we have to associate a employeeID (using its primary key) to create that new entry.

**Not acceptable new Row**
| EmployeeID | SalesPerson |  SalesOffice | Number |Customer 1 | Customer 2 | Customer 3 | 
| -- | -- | -- | -- | -- | -- | -- | 
| ??? | ??? | Orlando | 1234-56789  | |  |

#### Update Anomaly
Since or db is not normalized, there is data redundancy among or rows. If some value have to be updated, then multiples updates will be made. If we don’t update all rows, then inconsistencies appear.

| EmployeeID | SalesPerson |  SalesOffice | Number |Customer 1 | Customer 2 | Customer 3 |  
| -- | -- | -- | -- | -- | -- | -- | 
|1003  | Mary | Chicago | **1234-56789** | Ford | GM  |  |
| 1004 | Joo | NY | 98765-4321 | Dell | HP | Aple
| 1005 | Martin | Chicago  | **1234-56789**  | Boing |  |

If the number of some salesoffice were updated, so every row that has it as value must be updated.

**Key words - Problems**: data duplication, data update issues, and increased effort to query data. 

#### Deletion Anomaly
Deletion of a row can cause removal of more than one set of facts. For instance, if Martin retires, then deleting that row cause us to lose information about the New York office.


## Normalization Rule
### First Normal Form (1NF)
The information is stored in a relational table with each column containing atomic values. There are no repeating groups of columns.

### Second Normal Form (2NF)
The table is in first normal form and all the columns depend on the table’s primary key.

### Third Normal Form (3NF)
the table is in second normal form and all of its columns are not transitively dependent on the primary key.

### Boyce and Codd Normal Form (BCNF)
### Fourth Normal Form (4NF)


## References

 - [Studytonight Database Mormalization](https://www.studytonight.com/dbms/database-normalization.php)
 - [Medium Normalização em Bancos de Dado - By Diego Machado](https://medium.com/@diegobmachado/normaliza%C3%A7%C3%A3o-em-banco-de-dados-5647cdf84a12)
 - [EssentialSQL - Database Normalization](https://www.essentialsql.com/get-ready-to-learn-sql-database-normalization-explained-in-simple-english/)

