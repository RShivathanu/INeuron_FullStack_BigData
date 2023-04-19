# Over Clause

### Only aggregated column should be present in group by query, Non-aggregated columns present in the query gives error in output.

### Create table employee

```sql
CREATE TABLE employee (
    firstname varchar(20),
    lastname varchar(20),
    age int,
    salary int,
    location varchar(20)
);
```

### Insert data into the table employee

```sql
insert into employee values ('sachin','tendulkar',28,10000,'bangalore');
insert into employee values ('shane','warne',30,20000,'bangalore');
insert into employee values ('rohit','sharma',32, 30000, 'hyderabad');
insert into employee values ('shikhar','dhawan',32,25000,'hyderabad');
insert into employee values ( 'rahul','dravid',31,20000,'bangalore');
insert into employee values ('saurabh','ganguly', 32, 15000,'pune');
insert into employee values ('kapil','dev',34,10000,'pune');
```
### write a query to get the following output

<pre>
+-----------+-----------+-----------+-------------+------------+
| firstname | lastname  | location  | total_count | avg_salary |
+-----------+-----------+-----------+-------------+------------+
| sachin    | tendulkar | bangalore |           3 | 16666.6667 |
| shane     | warne     | bangalore |           3 | 16666.6667 |
| rohit     | sharma    | hyderabad |           2 | 27500.0000 |
| shikhar   | dhawan    | hyderabad |           2 | 27500.0000 |
| rahul     | dravid    | bangalore |           3 | 16666.6667 |
| saurabh   | ganguly   | pune      |           2 | 12500.0000 |
| kapil     | dev       | pune      |           2 | 12500.0000 |
+-----------+-----------+-----------+-------------+------------+
</pre>

```sql
select firstname, lastname, location, count(location), avg(salary) from employee group by location;
```
<pre>
mysql> select firstname, lastname, location, count(location), avg(salary) from employee group by location;
ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'trendytech.employee.firstname' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
</pre>

> It will gives the above error

### By using joins we can get the output, but is not the optimal approach.

```sql
select firstname, lastname, employee.location,
total_count, avg_salary from employee join (select location, count(location) as total_count, avg(salary) as
avg_salary from employee group by location) temptable on
employee.location = temptable.location;
```
<pre>
+-----------+-----------+-----------+-------------+------------+
| firstname | lastname  | location  | total_count | avg_salary |
+-----------+-----------+-----------+-------------+------------+
| sachin    | tendulkar | bangalore |           3 | 16666.6667 |
| shane     | warne     | bangalore |           3 | 16666.6667 |
| rohit     | sharma    | hyderabad |           2 | 27500.0000 |
| shikhar   | dhawan    | hyderabad |           2 | 27500.0000 |
| rahul     | dravid    | bangalore |           3 | 16666.6667 |
| saurabh   | ganguly   | pune      |           2 | 12500.0000 |
| kapil     | dev       | pune      |           2 | 12500.0000 |
+-----------+-----------+-----------+-------------+------------+
</pre>

> Output using **Joins**.

### we can use OVER PARTITION BY to achieve this easily

```sql
select firstname, lastname, location,
count(location) OVER (PARTITION BY location) as total,
avg(salary) OVER (PARTITION BY location) as average
from employee;
```
<pre>
+-----------+-----------+-----------+-------+------------+
| firstname | lastname  | location  | total | average    |
+-----------+-----------+-----------+-------+------------+
| sachin    | tendulkar | bangalore |     3 | 16666.6667 |
| shane     | warne     | bangalore |     3 | 16666.6667 |
| rahul     | dravid    | bangalore |     3 | 16666.6667 |
| rohit     | sharma    | hyderabad |     2 | 27500.0000 |
| shikhar   | dhawan    | hyderabad |     2 | 27500.0000 |
| saurabh   | ganguly   | pune      |     2 | 12500.0000 |
| kapil     | dev       | pune      |     2 | 12500.0000 |
+-----------+-----------+-----------+-------+------------+
</pre>

* This is the output for the above Over and partition query.
* Using **OVER CLAUSE** we came to know that over is the optimal approach for the above problem.