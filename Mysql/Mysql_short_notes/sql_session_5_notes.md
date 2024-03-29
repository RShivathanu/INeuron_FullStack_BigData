# Foreign Key Constraint complete demonstration

> Here, we already created employee table
```sql
desc employee;
```
<pre>
+-----------+-------------+------+-----+-----------+-------+
| Field     | Type        | Null | Key | Default   | Extra |
+-----------+-------------+------+-----+-----------+-------+
| id        | int         | NO   | PRI | NULL      |       |
| firstname | varchar(40) | YES  |     | NULL      |       |
| lastname  | varchar(20) | NO   |     | NULL      |       |
| age       | int         | NO   |     | NULL      |       |
| salary    | int         | NO   |     | NULL      |       |
+-----------+-------------+------+-----+-----------+-------+
</pre>
> I have added the column dept_id to the current employee table
```sql
alter table employee add column dept_id int;
desc employee;
```
<pre>
+-----------+-------------+------+-----+-----------+-------+
| Field     | Type        | Null | Key | Default   | Extra |
+-----------+-------------+------+-----+-----------+-------+
| id        | int         | NO   | PRI | NULL      |       |
| firstname | varchar(40) | YES  |     | NULL      |       |
| lastname  | varchar(20) | NO   |     | NULL      |       |
| age       | int         | NO   |     | NULL      |       |
| salary    | int         | NO   |     | NULL      |       |
| location  | varchar(20) | NO   |     | bangalore |       |
| dept_id   | int         | YES  |     | NULL      |       |
+-----------+-------------+------+-----+-----------+-------+
</pre>
> Created a new parent table to show foreign key constraint
```sql
create table department (id int primary key, name varchar(10), location varchar(10));

desc department;
```
> below is the department table
<pre>
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | NO   | PRI | NULL    |       |
| name     | varchar(10) | YES  |     | NULL    |       |
| location | varchar(10) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
</pre>

```sql
select * from department;
```
<pre>
+----+------+----------+
| id | name | location |
+----+------+----------+
|  1 | cse  | bglore   |
|  2 | ece  | bglore   |
|  3 | mech | bglore   |
+----+------+----------+
</pre>
> below is the records inserted

```sql
alter table employee add constraint fk_dept foreign key(dept_id) REFERENCES department(id);
```
> Here, we added the FK constraint to the employee(child) table
<pre>
+-----------+-------------+------+-----+-----------+-------+
| Field     | Type        | Null | Key | Default   | Extra |
+-----------+-------------+------+-----+-----------+-------+
| id        | int         | NO   | PRI | NULL      |       |
| firstname | varchar(40) | YES  |     | NULL      |       |
| lastname  | varchar(20) | NO   |     | NULL      |       |
| age       | int         | NO   |     | NULL      |       |
| salary    | int         | NO   |     | NULL      |       |
| location  | varchar(20) | NO   |     | bangalore |       |
| dept_id   | int         | YES  | MUL | NULL      |       |
+-----------+-------------+------+-----+-----------+-------+
</pre>

```sql
mysql> INSERT INTO employee (id,firstname,lastname,age,salary,dept_id) VALUES (1,'naveen','pattai',24,20000,1);
Query OK, 1 row affected (0.02 sec)

mysql> INSERT INTO employee (id,firstname,lastname,age,salary,dept_id) VALUES (2,'naresh','kumar',24,20000,2);
Query OK, 1 row affected (0.04 sec)
```
> Here, we inserted 2 records successfully

> While entering 3rd record we got an error
```sql
mysql> INSERT INTO employee (id,firstname,lastname,age,salary,dept_id) VALUES (3,'akshar','kumar',24,20000,4);
```

ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`trendytech`.`employee`, CONSTRAINT `fk_dept` FOREIGN KEY (`dept_id`) REFERENCES `department` (`id`))

> Here, we got because fk constraint
> we inserted the recored i.e dept_id = 4 that is not in the list
> I conclude we can't insert data that is not present in the parent class(department)

* The foreign key constraint is used to prevent actions that would destroy links between two tables.
* A foreign key is a field in one table that refers to the primary key in another table.
* dept_id is a foreign key in employee table which refers to id(primary key) in department table.
* The table with the foreign key is called the child table.
* The table with primary key is called the parent or referenced table.
---

* Constraints are used to limit the type of data that can go into a table.
* This ensures the accuracy and reliability of the data is maintained.
* If there is any violation then the action is aborted
---

# session - 7 

> Below is the employee table

<pre>
+----+-----------+----------+-----+--------+---------+
| id | firstname | lastname | age | salary | dept_id |
+----+-----------+----------+-----+--------+---------+
|  1 | naveen    | pattai   |  24 |  20000 |       1 |
|  2 | naresh    | kumar    |  24 |  20000 |       2 |
|  3 | naveen    | pattai   |  24 |  20000 |    NULL |
|  4 | praveen   | pattai   |  24 |  20000 |    NULL |
|  5 | kavya     | pattai   |  24 |  20000 |    NULL |
|  6 | akshar    | pattai   |  24 |  20000 |    NULL |
+----+-----------+----------+-----+--------+---------+
</pre>

```sql
mysql> select DISTINCT firstname from employee  order by id desc;
```
> ERROR 3065 (HY000): Expression #1 of ORDER BY clause is not in SELECT list, references column 'trendytech.employee.id' which is not in SELECT list; this is incompatible with DISTINCT

> The above query gives error because we are doing **DISTNICT AND ORDER**  in the same query.
we will discuss about this further

> To discuss about this we have to know **order of execution**.

```sql
SELECT firstname FROM employee;
```
* The **order of execution** for above query is below.
    
        -> FROM (LOADING THE TABLE)
        -> SELECT (PROJECTING firstname)
---

> Below is the query and table to show order of execution for order by 

```SQL
SELECT firstname FROM employee order by id desc;
```
<pre>
+-----------+
| firstname |
+-----------+
| akshar    |
| kavya     |
| praveen   |
| naveen    |
| naresh    |
| naveen    |
+-----------+</pre>
* The **order of execution** for ***ORDER BY***.

1. FROM (LOADING THE TABLE)
        
        select * from employee;

2. SELECT (PROJECTING firstname, id )
        
        select firstname, id FROM employee;

> Here, In main select state we didnt selected column **id** for projection, but we mentioned that query should **Order By column id**. So without **column id** we can't use orderBy.

> In proactive way, the system automatically projects the id column with firstname like mentioned below.
        
        select firstname, id FROM employee;

3. Order By (based on **column id** it will Order By)

        SELECT firstname, id FROM employee ORDER BY id;

> From here, after execution of order by **id column will be removed or discarded from the projection** only the below select statement will be displayed.

        SELECT firstname FROM employee;
---

## Using DISTINCT and ORDER BY in same Query

```sql
SELECT DISTINCT firstname FROM employee order by id desc;
```
> ERROR 3065 (HY000): Expression #1 of ORDER BY clause is not in SELECT list, references column 'trendytech.employee.id' which is not in SELECT list; this is incompatible with DISTINCT

###  Why it is not working??

> Let us see the source/order of execution for above statement.

1. FROM (LOADING THE TABLE)
        
        select * from employee;

2. SELECT (PROJECTING firstname, id )
        
        select firstname, id FROM employee;

1. DISTINCT 

       SELECT DISTINCT firstname from employee;
<pre>
+-----------+
| firstname |
+-----------+
| naveen    |
| naresh    |
| praveen   |
| kavya     |
| akshar    |
+-----------+
</pre>

        SELECT DISTINCT firstname, id FROM employee;
<pre>
+-----------+----+
| firstname | id |
+-----------+----+
| naveen    |  1 |
| naresh    |  2 |
| naveen    |  3 |
| praveen   |  4 |
| kavya     |  5 |
| akshar    |  6 |
+-----------+----+       
</pre>

* > Here, **DISTINCT** works on combination of 2 columns(firstname,id).

* > So the result will not be accurate.

* > Check the above 2 table mentioned, the result varies we used **DISTINCT** in both the table, but the both tables result is different.

* > So, logically what we are trying to achieve went wrong.

* > Sql should not give wrong output, That's why **sql is showing an error** while using **DISTINCT & ORDER BY** in the same statement.

* > The query and error mentioned below.


```sql
SELECT DISTINCT firstname FROM employee order by id desc;
```
> ERROR 3065 (HY000): Expression #1 of ORDER BY clause is not in SELECT list, references column 'trendytech.employee.id' which is not in SELECT list; this is incompatible with DISTINCT



4. ORDER BY (based on **column id** it will Order By)

         SELECT firstname, id FROM employee ORDER BY id;
---
