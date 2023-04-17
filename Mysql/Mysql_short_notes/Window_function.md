# Window Function

### create table shop_sales_data and insert data into that table

```sql
create table shop_sales_data
(
sales_date date,
shop_id varchar(5),
sales_amount int
);
```

* INSERT ROWS INTO THE TABLE
```sql
insert into shop_sales_data values('2022-02-14','S1',200);
insert into shop_sales_data values('2022-02-15','S1',300);
insert into shop_sales_data values('2022-02-14','S2',600);
insert into shop_sales_data values('2022-02-15','S3',500);
insert into shop_sales_data values('2022-02-18','S1',400);
insert into shop_sales_data values('2022-02-17','S2',250);
insert into shop_sales_data values('2022-02-20','S3',300);
```
<pre>
+------------+---------+--------------+-------------+
| sales_date | shop_id | sales_amount | total_sales |
+------------+---------+--------------+-------------+
| 2022-02-14 | S1      |          200 |         200 |
| 2022-02-15 | S1      |          300 |         500 |
| 2022-02-18 | S1      |          400 |         900 |
| 2022-02-14 | S2      |          600 |         600 |
| 2022-02-17 | S2      |          250 |         850 |
| 2022-02-15 | S3      |          500 |         500 |
| 2022-02-20 | S3      |          300 |         800 |
+------------+---------+--------------+-------------+
</pre>

```sql
select *, 
sum(sales_amount) over(partition by shop_id order by sales_date)
as 
total_sales 
from 
shop_sales_data;
```

> Total count of sales for each shop using window function
> Working functions - SUM(), MIN(), MAX(), COUNT(), AVG()

* If we only use Order by In Over Clause
```
select *,
       sum(sales_amount) over(order by sales_amount desc) as running_sum_of_sales
from shop_sales_data;
```

> Here, sum func will be run on sales_amount column without partition it will be order by sales_amount column.
<pre>
+------------+---------+--------------+----------------------+
| sales_date | shop_id | sales_amount | running_sum_of_sales |
+------------+---------+--------------+----------------------+
| 2022-02-14 | S2      |          600 |                  600 |
| 2022-02-15 | S3      |          500 |                 1100 |
| 2022-02-18 | S1      |          400 |                 1500 |
| 2022-02-15 | S1      |          300 |                 2100 |
| 2022-02-20 | S3      |          300 |                 2100 |
| 2022-02-17 | S2      |          250 |                 2350 |
| 2022-02-14 | S1      |          200 |                 2550 |
+------------+---------+--------------+----------------------+
</pre>

> If we only use Partition By

```
select *,
       sum(sales_amount) over(partition by shop_id) as total_sum_of_sales
from shop_sales_data;
```
<pre>
+------------+---------+--------------+--------------------+
| sales_date | shop_id | sales_amount | total_sum_of_sales |
+------------+---------+--------------+--------------------+
| 2022-02-14 | S1      |          200 |                900 |
| 2022-02-15 | S1      |          300 |                900 |
| 2022-02-18 | S1      |          400 |                900 |
| 2022-02-14 | S2      |          600 |                850 |
| 2022-02-17 | S2      |          250 |                850 |
| 2022-02-15 | S3      |          500 |                800 |
| 2022-02-20 | S3      |          300 |                800 |
+------------+---------+--------------+--------------------+
</pre>

> If we only use Partition By & Order By together
```
select *,
       sum(sales_amount) over(partition by shop_id order by sales_amount desc) as running_sum_of_sales
from shop_sales_data;
```
<pre>
+------------+---------+--------------+----------------------+
| sales_date | shop_id | sales_amount | running_sum_of_sales |
+------------+---------+--------------+----------------------+
| 2022-02-18 | S1      |          400 |                  400 |
| 2022-02-15 | S1      |          300 |                  700 |
| 2022-02-14 | S1      |          200 |                  900 |
| 2022-02-14 | S2      |          600 |                  600 |
| 2022-02-17 | S2      |          250 |                  850 |
| 2022-02-15 | S3      |          500 |                  500 |
| 2022-02-20 | S3      |          300 |                  800 |
+------------+---------+--------------+----------------------+
</pre>

```sql
select *,
       sum(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_sum_of_sales,
       avg(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_avg_of_sales,
       max(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_max_of_sales,
       min(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_min_of_sales
from shop_sales_data;
```

<pre>
+------------+---------+--------------+-------+----------+-------+-------+
| sales_date | shop_id | sales_amount | sum_s | avg_s    | max_s | min_s |
+------------+---------+--------------+-------+----------+-------+-------+
| 2022-02-18 | S1      |          400 |   400 | 400.0000 |   400 |   400 |
| 2022-02-15 | S1      |          300 |   700 | 350.0000 |   400 |   300 |
| 2022-02-14 | S1      |          200 |   900 | 300.0000 |   400 |   200 |
| 2022-02-17 | S2      |          250 |   250 | 250.0000 |   250 |   250 |
| 2022-02-14 | S2      |          600 |   850 | 425.0000 |   600 |   250 |
| 2022-02-20 | S3      |          300 |   300 | 300.0000 |   300 |   300 |
| 2022-02-15 | S3      |          500 |   800 | 400.0000 |   500 |   300 |
+------------+---------+--------------+-------+----------+-------+-------+
</pre>

---

### create table amazon_sales_data

```sql
create table amazon_sales_data
(
    sales_date date,
    sales_amount int
);
```
```sql
insert into amazon_sales_data values('2022-08-21',500);
insert into amazon_sales_data values('2022-08-22',600);
insert into amazon_sales_data values('2022-08-19',300);
insert into amazon_sales_data values('2022-08-18',200);
insert into amazon_sales_data values('2022-08-25',800);
```

### Query - Calculate the date wise rolling average of amazon sales

```sql
select *,
    FORMAT (avg(sales_amount) over(order by sales_date) ,2) as rolling_avg
    from amazon_sales_data;
```
<pre>
+------------+--------------+-------------+
| sales_date | sales_amount | rolling_avg |
+------------+--------------+-------------+
| 2022-08-18 |          200 | 200.00      |
| 2022-08-19 |          300 | 250.00      |
| 2022-08-21 |          500 | 333.33      |
| 2022-08-22 |          600 | 400.00      |
| 2022-08-25 |          800 | 480.00      |
+------------+--------------+-------------+
</pre>

* Above table displays the output of rolling_avg using window function.
* Here, average is updated after each row step by step.


```sql
select *,
       FORMAT (avg(sales_amount) over(order by sales_date),2) 
       as rolling_avg,
       sum(sales_amount) over(order by sales_date) 
       as rolling_sum
from amazon_sales_data;
```
* In these statetment both average and sum of all sales amount in the table is displayed using window function.
* Here, I used **FORMAT** function to round the result data into 2 decimal places.

<pre>
+------------+--------------+-------------+-------------+
| sales_date | sales_amount | rolling_avg | rolling_sum |
+------------+--------------+-------------+-------------+
| 2022-08-18 |          200 |    200.0000 |         200 |
| 2022-08-19 |          300 |    250.0000 |         500 |
| 2022-08-21 |          500 |    333.3333 |        1000 |
| 2022-08-22 |          600 |    400.0000 |        1600 |
| 2022-08-25 |          800 |    480.0000 |        2400 |
+------------+--------------+-------------+-------------+
</pre>
---

## Rank(), Row_Number(), Dense_Rank() window functions

```sql
insert into shop_sales_data values('2022-02-19','S1',400);
insert into shop_sales_data values('2022-02-20','S1',400);
insert into shop_sales_data values('2022-02-22','S1',300);
insert into shop_sales_data values('2022-02-25','S1',200);
insert into shop_sales_data values('2022-02-15','S2',600);
insert into shop_sales_data values('2022-02-16','S2',600);
insert into shop_sales_data values('2022-02-16','S3',500);
insert into shop_sales_data values('2022-02-18','S3',500);
insert into shop_sales_data values('2022-02-19','S3',300);
```

```sql
select *,
       row_number() over(partition by shop_id order by sales_amount desc) as row_num,
       rank() over(partition by shop_id order by sales_amount desc) as rank_val,
       dense_rank() over(partition by shop_id order by sales_amount desc) as dense_rank_val
from shop_sales_data;
```

* In window functions there are 3 main window functions they are 
    * Row_number()
    * Rank()
    * Dense_Rank()


> If you want to know more about window func refer this below link.

https://sparkbyexamples.com/spark/difference-in-dense_rank-and-row_number-in-spark/

```sql
create table employees
(
    emp_id int,
    salary int,
    dept_name VARCHAR(30)

);
```
```sql
insert into employees values(1,10000,'Software');
insert into employees values(2,11000,'Software');
insert into employees values(3,11000,'Software');
insert into employees values(4,11000,'Software');
insert into employees values(5,15000,'Finance');
insert into employees values(6,15000,'Finance');
insert into employees values(7,15000,'IT');
insert into employees values(8,12000,'HR');
insert into employees values(9,12000,'HR');
insert into employees values(10,11000,'HR');
```


### Query - get one employee from each department who is getting maximum salary (employee can be random if salary is same)

```sql
select 
    tmp.*
    from (
        select *,
        row_number() over(partition by dept_name order by salary desc) as row_num
        from employees) as tmp
where tmp.row_num = 1;
```
<pre>
+--------+--------+-----------+---------+
| emp_id | salary | dept_name | row_num |
+--------+--------+-----------+---------+
|      5 |  15000 | Finance   |       1 |
|      8 |  12000 | HR        |       1 |
|      7 |  15000 | IT        |       1 |
|      2 |  11000 | Software  |       1 |
+--------+--------+-----------+---------+
</pre>


### Query - get all employees from each department who are getting maximum salary
```sql
select tmp.*
from (
    select *,
        rank() over(partition by dept_name order by salary desc) as rank_num
    from employees) tmp
where tmp.rank_num = 1;
```
<pre>
+--------+--------+-----------+----------+
| emp_id | salary | dept_name | rank_num |
+--------+--------+-----------+----------+
|      5 |  15000 | Finance   |        1 |
|      6 |  15000 | Finance   |        1 |
|      8 |  12000 | HR        |        1 |
|      9 |  12000 | HR        |        1 |
|      7 |  15000 | IT        |        1 |
|      2 |  11000 | Software  |        1 |
|      3 |  11000 | Software  |        1 |
|      4 |  11000 | Software  |        1 |
+--------+--------+-----------+----------+
</pre>

* Here, I used rank() to get all the employees getting high salary.
---

### Query - get all top 2 ranked employees from each department who are getting maximum salary

```sql
select 
    tmp.*
from (select *,
        dense_rank() over(partition by dept_name order by salary desc) as dense_rank_num
    from employees) tmp
where tmp.dense_rank_num <= 2;
```
<pre>
+--------+--------+-----------+----------------+
| emp_id | salary | dept_name | dense_rank_num |
+--------+--------+-----------+----------------+
|      5 |  15000 | Finance   |              1 |
|      6 |  15000 | Finance   |              1 |
|      8 |  12000 | HR        |              1 |
|      9 |  12000 | HR        |              1 |
|     10 |  11000 | HR        |              2 |
|      7 |  15000 | IT        |              1 |
|      2 |  11000 | Software  |              1 |
|      3 |  11000 | Software  |              1 |
|      4 |  11000 | Software  |              1 |
|      1 |  10000 | Software  |              2 |
+--------+--------+-----------+----------------+
</pre>


### Example for lag and lead

```sql
create table daily_sales
(
sales_date date,
sales_amount int
);
```

```sql
insert into daily_sales values('2022-03-11',400);
insert into daily_sales values('2022-03-12',500);
insert into daily_sales values('2022-03-13',300);
insert into daily_sales values('2022-03-14',600);
insert into daily_sales values('2022-03-15',500);
insert into daily_sales values('2022-03-16',200);
```

select * from daily_sales;

select *,
      lag(sales_amount, 1) over(order by sales_date) as pre_day_sales
from daily_sales;

# we can use this to replace null with defualt value like 0
select *,
	coalesce(lag(sales_amount,1) over(order by sales_date), 0) as prev_sales
from daily_sales;

# Query - Calculate the differnce of sales with previous day sales
# Here null will be derived
select sales_date,
       sales_amount as curr_day_sales,
       lag(sales_amount, 1) over(order by sales_date) as prev_day_sales,
       sales_amount - lag(sales_amount, 1) over(order by sales_date) as sales_diff
from daily_sales;

# Here we can replace null with 0
select sales_date,
       sales_amount as curr_day_sales,
       lag(sales_amount, 1, 0) over(order by sales_date) as prev_day_sales,
       sales_amount - lag(sales_amount, 1, 0) over(order by sales_date) as sales_diff
from daily_sales;

# Diff between lead and lag
select *,
      lag(sales_amount, 1) over(order by sales_date) as pre_day_sales
from daily_sales;

select *,
      lead(sales_amount, 1) over(order by sales_date) as next_day_sales
from daily_sales;

create table employees(
  emp_id int,
  emp_name varchar(50),
  mobile BIGINT,
  dept_name varchar(50),
  salary int 
);

insert into employees values(1,'Shashank',778768768,'Software',1000);
insert into employees values(2,'Rahul',876778877,'IT',2000);
insert into employees values(3,'Amit',098798998,'HR',5000);

insert into employees values(4,'Nikhil',67766767,'IT',3000);

select * from employees;

--- Create views in SQL
create view employee_data_for_finance as select emp_id, emp_name,salary from employees;

select * from employee_data_for_finance;

--- Create logic for department wise salary sum
create view department_wise_salary as select dept_name, sum(salary) from employees group by dept_name;

drop view department_wise_salary;

create view department_wise_salary as select dept_name, sum(salary) as total_salary from employees group by dept_name;

