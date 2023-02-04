# Foreign Key Constraint complete demomenstration

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