# Sql Session 4
## Update,delete,Alter,DDL vs DMl, truncate vs delete

### CRUD OPERATIONS

create - Insert
read - select
update - update
delete - delete

```sql
CREATE TABLE employee (
    id int PRIMARY KEY,
    firstname varchar(20) NOT NULL,
    middlename varchar(20),
    lastname varchar(20) NOT NULL,
    age int NOT NULL,
    salary int NOT NULL,
    location varchar(20) NOT NULL DEFAULT 'bangalore'
);

INSERT INTO employee (id,firstname,lastname,age,salary) VALUES (1,'naveen','pattai',24,20000);

mysql> INSERT INTO employee (id,firstname,lastname,age,salary) VALUES (1,'naveen','pattai',24,20000);
Query OK, 1 row affected (0.01 sec)

mysql> INSERT INTO employee (id,firstname,lastname,age,salary) VALUES (2,'naresh','kumar',23,20000);
Query OK, 1 row affected (0.03 sec)

mysql> INSERT INTO employee (id,firstname,lastname,age,salary) VALUES (3,'akshar','SP',23,20000);

select firstname from employee  where firstName = 'Naveen';
```

* **note-** by default data in database is case insensitive.
> if u want to change to case sensitive add binary to the query

```sql
select firstname from employee  where binary firstname = 'naveen';
```

 ### UPDATE QUERY
> * Used to update the records in the table.
> * Set keyword is used to update
> * where clause is used to update the particular row

```sql
select * from employee;
```
<pre>
+----+-----------+------------+----------+-----+--------+-----------+
| id | firstname | middlename | lastname | age | salary | location  |
+----+-----------+------------+----------+-----+--------+-----------+
|  1 | naveen    | NULL       | chandru  |  24 |  20000 | bangalore |
|  2 | naresh    | NULL       | kumar    |  23 |  20000 | bangalore |
|  3 | akshar    | NULL       | SP       |  23 |  20000 | bangalore |
+----+-----------+------------+----------+-----+--------+-----------+
</pre>

```sql
update employee set lastname = 'mindri' where lastname = 'kumar';
```
<pre>
+----+-----------+------------+----------+-----+--------+-----------+
| id | firstname | middlename | lastname | age | salary | location  |
+----+-----------+------------+----------+-----+--------+-----------+
|  1 | naveen    | NULL       | chandru  |  24 |  20000 | bangalore |
|  2 | naresh    | NULL       | mindri   |  23 |  20000 | bangalore |
|  3 | akshar    | NULL       | SP       |  23 |  20000 | bangalore |
+----+-----------+------------+----------+-----+--------+-----------+
</pre>
> * To increase the salary of the performing employee
```sql
update employee set salary = salary * 2 where age = 24;
```
> * Here, the performer will get 100% Hike
---
### DELETE QUERY
> * Delete keyword is used to delete a particular row or column
> * Here, mention the correct where clause or else entire table data will be deleted.
```sql
DELETE FROM employee WHERE ID = 1;
```
> * The entire record where id = 1, will be deleted.
---
### ALTER COMMAND
> * ALter is to alter the structure of the table.

```sql
alter table employee add column education varchar(10);
```
> * added the column educatiion into the table

```sql
alter table employee drop column education;
```
 > * used to drop the particular column

```sql
alter table employee modify column firstname varchar(40);
```
> * used to change the storage for datatype saved before
```sql
alter table employee drop primary key;
```
> * used to drop the primary key

```sql
alter table employee add primary key(id);
```
> * used to add the primary key to any column
---

### DDL VS DML

> * Data defination language - It deals with only the structured of the table.
> * Data manipulation language - It deals with the data directly.

insert, update, delete - DML commands

```sql
delete from employee;
```
* >deletes all the record from the table.


**Truncate** keyword also removes all the record from the table.

### TRUNCATE VS DELETE
* Delete(DML) will deletes the data individually one by one in the database.
* **Truncates(DDL)** internally drops the table and re-creates the table. That's why it is called ddl command.

**Note** > If u have billions of record, You use truncate rather than delete because delete will take lot of time compared to truncate. 




