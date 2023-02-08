# CASE STATEMENTS

> The CASE expression goes through conditions and returns a value when the first condition is met (like an if-then-else statement). 

> So, once a condition is true, it will stop reading and return the result. 

> If no conditions are true, it returns the value in the ELSE clause.

## CASE Syntax

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```
> Below is the sample table for practice

```sql
select * from courses;
```
<pre>
+-----------+-----------------+------------------------+------------+---------------------+
| course_id | course_name     | course_duration_months | course_fee | changed_at          |
+-----------+-----------------+------------------------+------------+---------------------+
|         1 | big data        |                    6.0 |      50000 | 2023-02-08 00:25:52 |
|         2 | web development |                    3.0 |      40000 | 2023-02-08 00:28:28 |
|         3 | data science    |                    4.0 |      50000 | 2023-02-08 00:25:52 |
|         4 | devops          |                    5.0 |      30000 | 2023-02-08 00:25:52 |
+-----------+-----------------+------------------------+------------+---------------------+
</pre>

> If a course is more than 4 months we categorize it as masters program else it is a diploma.

```sql
select course_id, course_name, course_fee,
CASE
WHEN course_duration_months >= 4 THEN 'masters'
ELSE 'diploma'
END as course_type
from courses;
```
<pre>
+-----------+-----------------+------------+-------------+
| course_id | course_name     | course_fee | course_type |
+-----------+-----------------+------------+-------------+
|         1 | big data        |      50000 | masters     |
|         2 | web development |      40000 | diploma     |
|         3 | data science    |      50000 | diploma     |
|         4 | devops          |      30000 | masters     |
+-----------+-----------------+------------+-------------+
</pre>
---

> If a course_fee > 40000 categorize as expensive else categorize as affordable.

```sql
select course_id, course_name , course_duration_months,
CASE 
WHEN course_fee > 40000 THEN 'Expensive'
ELSE 'Affordable'
END as fee_type
from courses;
```

<pre>
+-----------+-----------------+------------------------+------------+
| course_id | course_name     | course_duration_months | fee_type   |
+-----------+-----------------+------------------------+------------+
|         1 | big data        |                    6.0 | Expensive  |
|         2 | web development |                    3.0 | Affordable |
|         3 | data science    |                    4.0 | Expensive  |
|         4 | devops          |                    5.0 | Affordable |
+-----------+-----------------+------------------------+------------+
</pre>
---

> If people working for walmart, flipkart, microsoft we want to say product based and all others service based.

```sql
select student_fname, student_lname, student_company,
CASE
WHEN student_company IN ('flipkart', 'walmart', 'microsoft') THEN 'Product_based'
ELSE 'Service_based'
END as company_type
from students;
```
<pre>
+---------------+---------------+-----------------+---------------+
| student_fname | student_lname | student_company | company_type  |
+---------------+---------------+-----------------+---------------+
| rohit         | sharma        | walmart         | Product_based |
| virat         | kohli         | flipkart        | Product_based |
| shikhar       | dhawan        | NULL            | Service_based |
| rahul         | dravid        | walmart         | Product_based |
| kapil         | dev           | microsoft       | Product_based |
| brian         | lara          | tcs             | Service_based |
| carl          | hooper        | wipro           | Service_based |
| saurabh       | ganguly       | wipro           | Service_based |
+---------------+---------------+-----------------+---------------+
</pre>

