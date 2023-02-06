# Aggregate functions

### What is aggregate??

> Clubing few things and getting the output.

> Getting the count of items in table.

> Finding the average runs scored a by a player etc...

    SELECT COUNT(*) FROM employee;

<pre>
+----------+
| COUNT(*) |
+----------+
|        6 |
+----------+
</pre>
---

## Group By

> We want to group the data based on logic.

> We can group the data based on the company details, like all the people working in tcs are grouped together, working in walmart are grouped together.

> All the employees who earns more than 20000 are grouped together. 

#### For demonstration we will use City table described below :

    DESC City;

<pre>
+-------------+------+------+-----+---------+
| Field       | Type | Null | Key | Default |
+-------------+------+------+-----+---------+
| ID          | int  | YES  |     | NULL    |       
| NAME        | text | YES  |     | NULL    |      
| COUNTRYCODE | text | YES  |     | NULL    |       
| DISTRICT    | text | YES  |     | NULL    |       
| POPULATION  | text | YES  |     | NULL    |   
+-------------+------+------+-----+---------+
</pre>

> **Note-** If you having something as part of a grouping coulumn, It should be in your select or else it will show the error.

> We will look into an example below.

```sql
SELECT COUNTRYCODE , COUNT(*) FROM City GROUP BY COUNTRYCODE ;
```

<pre>
+-------------+----------+
| COUNTRYCODE | count(*) |
+-------------+----------+
| NLD         |        2 |
| BRA         |        2 |
| BGR         |        1 |
| BDI         |        1 |
| CHL         |        1 |
| EGY         |        1 |
| SLV         |        1 |
| Dar         |        1 |
| PHL         |        2 |
+-------------+----------+
</pre>

> Here, In the above example we grouped based on the column 'CountryCode'.

> here, it also shows the count of data present in each country.    

    SELECT COUNTRYCODE , COUNT(*) FROM City GROUP BY DISTRICT;

> ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'employee.City.COUNTRYCODE' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

> The above query showing error because, we have to use same group by i.e **DISTRICT** in the select statement. 

    SELECT COUNTRYCODE, DISTRICT , COUNT(*) FROM City GROUP BY COUNTRYCODE, DISTRICT;

<pre>
+-------------+-------------------+----------+
| COUNTRYCODE | DISTRICT          | COUNT(*) |
+-------------+-------------------+----------+
| NLD         | Zuid-Holland      |        1 |
| NLD         | Noord-Holland     |        1 |
| BRA         | Rio Grande do Sul |        1 |
| BRA         | Bahia             |        1 |
| BGR         | Varna             |        1 |
| BDI         | Bujumbura         |        1 |
| CHL         | Santiago          |        1 |
| EGY         | al-Minya          |        1 |
| SLV         | Santa Ana         |        1 |
| Dar         | ETH Amhara        |        1 |
| PHL         | CAR               |        1 |
| PHL         | Southern Mindanao |        1 |
+-------------+-------------------+----------+
</pre>

> The above query shows the multiple column group by in the single statement.  
---

## MIN && MAX

> **MIN** - Used to find lowest value or minimum of the given data.

> **MAX** - Used to find highest value or maximum of the given data.

#### For demonstration we will use City table described below :

    DESC City;

<pre>
+-------------+------+------+-----+---------+
| Field       | Type | Null | Key | Default |
+-------------+------+------+-----+---------+
| ID          | int  | YES  |     | NULL    |       
| NAME        | text | YES  |     | NULL    |       
| COUNTRYCODE | text | YES  |     | NULL    |       
| DISTRICT    | text | YES  |     | NULL    |       
| POPULATION  | int  | YES  |     | NULL    |       
+-------------+------+------+-----+---------+
</pre>

> Below is exmp for the **MIN** aggregate function.

    SELECT MIN(POPULATION) as Lowest_Population FROM City;
<pre>
+-------------------+
| Lowest_Population |
+-------------------+
|             93232 |
+-------------------+
</pre>

> Below is exmp for the **MAX** aggregate function.

    SELECT MAX(POPULATION) as Highest_Population FROM City;
<pre>
+--------------------+
| Highest_Population |
+--------------------+
|            4703954 |
+--------------------+
</pre>

> ## Aggregate function using Group By

> #### MIN AND MAX
```sql
DESC order_details;
```
<pre>
+---------------+------+------+-----+---------+----------------+
| Field         | Type | Null | Key | Default | Extra          |
+---------------+------+------+-----+---------+----------------+
| OrderDetailID | int  | NO   | PRI | NULL    | auto_increment |
| OrderID       | int  | YES  | MUL | NULL    |                |
| ProductID     | int  | YES  | MUL | NULL    |                |
| Quantity      | int  | YES  |     | NULL    |                |
+---------------+------+------+-----+---------+----------------+
</pre>
```sql
SELECT ProductID, max(Quantity) FROM order_details 
GROUP BY ProductID 
Order BY ProductID 
limit 10;
```
<pre>
+-----------+---------------+
| ProductID | max(Quantity) |
+-----------+---------------+
|         1 |            45 |
|         2 |            60 |
|         3 |            50 |
|         4 |            35 |
|         5 |            65 |
|         6 |            30 |
|         7 |            15 |
|         8 |            70 |
|         9 |            20 |
|        10 |            30 |
+-----------+---------------+
</pre>

> #### MIN using GROUP BY 

```sql
SELECT ProductID, MIN(Quantity) FROM order_details 
GROUP BY ProductID 
Order BY ProductID 
limit 10;
```
<pre>
+-----------+---------------+
| ProductID | MIN(Quantity) |
+-----------+---------------+
|         1 |            10 |
|         2 |             7 |
|         3 |            30 |
|         4 |            10 |
|         5 |            12 |
|         6 |             6 |
|         7 |            10 |
|         8 |            70 |
|         9 |            20 |
|        10 |            15 |
+-----------+---------------+
</pre>
---