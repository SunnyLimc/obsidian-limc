## L1

- 相关的模型 最早在1970年被提出
	- 储存在简单的数据结构中
	- 用高级语言来访问数据
	- 物理空间留给根据具体的需求来实现

- 现在
	- 有了比较完善的 query scheduler
		- 就像当初编译器 vs 汇编一样

- 数据模型 解决数据如何储存
	- Relational
	- NoSQL
		- Key/Value
		- Graph
		- Document
		- Column-family
	- ML
		- Array/Matrix
	- Obsolete / Rare
		- Hierarchical
		- Network

- Relational Model:
	- unordered **set** of elements have attributes that represent instances of entities
	- refer a record in relational data model as a **tuple**
	- support varieties of data type and it's implement with many way in underlying
	- Values are (normally) atomic/scalar
	- NULL is a special member of every domain
	- [[Primary Key]]
	- [[Foreign Key]]
	
[[N-ary Storage Model (NSM)]]

- Data manipulation languages
	- Procedural - Relation Algebra
	- Non-Procedural - Relational Calculus
		- You have to do mathematical operation to decide the best optimization

- Procedural have basically 7 operation
	- select -> filter tuples
	- projection -> remap result in a scheduled way
	- union -> take two tuple with the **same type** and combine to one
	- intersection
	- difference	
	- produce -> concatenate one tuple and a one form another relation to produce a unique new tuple
	- join -> take two relation and filter the tuples that have the same key and values
	
- Extra Operators
	- Rename
	- Assignment
	- Duplicate Elimination
	- Aggregation
	- Sorting
	- Division

- Query Plan
  - the same high-level operation may have a completely different efficiency while underlying instructions are interpreted differently

## L2 (new)

for build relational algebra system for programming yesterday

SQL-92 to SQL:2016 standard

Relational Language SQL: DML, DDL, DCL, View definition, Integrity & Referential Constraints, Transactions

- type
	- set - unorder, no duplicates
	- list - defined order
	- bag - no order, allow duplicates

- aggregation: take multiple from **only the output list** for aggregating input to one
	- If you put it in input list, it doesn't make sense to constrain input
	- AVG
	- MIN
	- MAX
	- SUM
	- COUNT
		- `login (or another str)` the attr named 'login'
		- `*` all column(attributes)
		- `1 (or another NUM)`  a specific column index, and calculate the count of rows **NOT NULL** on that column
		- `DISTINCT login` only count for **unique** 'login'
	- multiple aggregation supported
- limitation for aggregation
	- use `GROUP BY` to collect extra information from table and avoid **single** confused and undefined output for the aggregation, due to no coordinate entry correspond to that aggregation. `GROUP BY` buck entity together and use those group to aggregate
- `HAVING` reference anything from your **output** list, whereas `WHERE` refer to the **input** list, **only apply for aggregate query**
- I you can peek ahead for SQL, like what DBMS needs is to **optimize query plan** and you'll get a shorter time. Like throw out the tuple not **in accordance with** the `HAVING` clause during I do `GROUP BY` or `AGGREGATE` computation. (NOT need to wait until handling the output list and do a bunch of wasted work)

**** The query should be defined as below: SELECT -> FROM -> JOIN -> WHERE -> GROUP BY -> HAVING -> ORDER BY ****

**** The sequence will be applied logically: FROM *named as input below* -> WHERE -> GROUP BY -> HAVING -> SELECT *named as output below* -> ORDER BY -> OFFSET, FETCH [(reference)](https://www.itprotoday.com/sql-server/logical-query-processing-what-it-and-what-it-means-you)****

- Basic Syntax
	- Compare: `WHERE age > 25`
	- Join: `SELECT s.name FROM enrolled AS e, student AS s WHERE e.grade ='A' AND e.cid = '15-721' AND e.sid = s.sid` and the last AND condition actually finish the JOIN

- case **sensitive** string
	- expect MySQL it's case insensitive
	- use **single quote** (some of them support double)
- **string matching**
	- use `LIKE` for matching
	- `%` **any** substring and included the **empty**
	- `_` **any one** character
- string operation
	- **allow** in **output** and **predicates**, not like aggregation, it almost everywhere
	- `SUBSTRING(str, pos, len)`
	- `UPPER`
	- `str1 || str2` is **standard** to concatenate, and `+` in MS SQL, and `CONCAT(str1, str2)` in MySQL
	- `INSTR(str, pattern)` return the first **pos** of pattern

- `IFNULL(expr, value)` specify another value if the value from expression is NULL

- DATE/TIME
	- **allow** in **output** and **predicates**, it almost everywhere
	- `NOW()` is **standard**, `CURRENT_TIMESTAMP` is various from func to keyword
	- `EXTRACT(1 FROM 2)` extract from field calculate date EXCEPT SQLite, subtract `-` `DATE(n)` ONLY correctly work in pavlo
		- use `julianday` in SQLite and subtraction and cast it to INT. (**most used**)

- DUMP result 
	- with `INTO`, you can dump your result to a **NOT already defined** table with same `#` of columns, is the **standard**
		- `SELECT DISTINCT cid INTO CourseIds FROM enrolled`
		- use `CREATE TABLE CourseIds (SELECT DISTINCT cid FROM enrolled)` in **MySQL**
	- with `insert`, you can append results to an existing table, **must** with the same column scheme
		- `INSERT INTO CourseIds (SELECT cid FROM enrolled)`
		- be aware of **violate** unique primary key principle

- **JOIN**
	- `LEFT OUTER JOIN .. [ON] <cond*>` take tuples on RIGHT and join to LEFT.
		- If the tuples not exist on right but on left, the missing attribute will be set to NULL. 
		- If the tuples not exist on left but on right, the tuple will be drop.
	- `RIGHT OUTER JOIN ... [ON] <cond*>` vice versa
	- `JOIN ... [ON] <cond*>` = `INNER JOIN ... [ON] <cond*>`, both should contains each or be drop
	- use **FROM** take the same result as **JOIN**: `FROM <tb*> WHERE <conds*>`

- **OUTPUT** Control
	- **sort output** by `ORDER BY <column*> [ASC|DESC]` and the default is ASC
		- no restriction **unlike** `group by`, it will find the needed data
	- `LIMIT` the counts returned to output
		- `LIMIT 20 OFFSET 10` you can set an offset
		- if the result is **unsorted**, the result is not guaranteed the same

- **nested** queries
	- Most DBMS would optimize it to **`JOIN`**, you won't double LOOP, cuz inner query should be **run once**
	- You have:
		- `ALL`, **lacking in SQLite**, consider `SELETE ... WHERE number >= (SELECT ... WHERE NOT EXISTS ( SELECT * FROM ... WHERE number < X)` as alternative
		- `<IS|NOT> EXISTS`, only return **one tuple (emphasize tuple)** if it exists, or NULL, generally used to **compare whole row**, use **`IN`** to compare attributes
		- `IN`, `SELECT name FROM stu WHERE sid IN ( SELECT ... )`, any tuple matching is acceptable
		- `ANY` = `IN`, the syntax is usually `WHERE sid = ANY( SELECT sid FROM xxx )`
		- EXIST vs IN ?
			- Use IN when the tuples is in a small bulk, use EXIST when it bulky, [ref link](https://asktom.oracle.com/pls/asktom/f?p=100:11:::::P11_QUESTION_ID:953229842074)
	- **allow almost everywhere**
		- like output statements: `SELECT ( SELECT S.name FROM stu AS S WHERE S.sid = E.sid ) AS sname FROM enrolled AS E WHERE cid = '15-445'`, can actually process the output
		- or input statements: `FROM`
	- handle aggregation without `GROUP BY`
		- `ALL` Math manipulation: `SELECT sid, name FROM student WHERE sid = > ALL( SELECT sid FROM enrolled )`
		- `IN` : with aggregation `SELECT sid, name FROM student WHERE sid IN ( SELECT MAX(sid) FROM enrolled )`, and with order by, `SELECT sid, name FROM student WHERE sid IN ( SELECT sid FROM enrolled ORDERED BY sid DESC LIMIT 1) `
		- `NOT EXIST`: `SELECT * FROM course WHERE NOT EXISTS( SELECT * FROM enrolled WHERE course.cid = enrolled.cid )`

- window function
	- Aggregation Func
	- Special window Func
		- `ROW_NUMBER()`: # (real index) of current row
		- `RANK()`: order position of the current row (**order followed by partition by**)
		- `LAG()`: grab previous rows
			- `LAG(expr, [offset, default)`
			- use `expr` to know which attribute to be used, simply is the column name
			- `offset` defaults to 1, the offset based on current row
			- `default` the value RETURN if the `expr` at `offset` is NULL, defaults to NULL
		- `NTILE()`: split tuples to group (by number and non-invasive)
			- `NTILE(expr)`, and return the value of group, `expr` is the divisor.
			- if divisor equals to 4, with 10 rows, it will be split to 3, 3, 2, 2 (and 3 is the first group)
			- will be applied to each `PARTITION BY` group individually
		- `OVER()`: **blank** do aggregation with function over all the results currently, **or specify** the **grouped** tuples
			- like `ROW_NUMBER() OVER(ORDER BY cid)`
		- `PARTITION BY()` to partition tuples and generate as a group of tuples (different from `group` which only generate one row)
			- `SELECT cid, sid, ROW_NUMBER() OVER (PARTITION BY cid) FROM enrolled ORDER BY cid`
			- partition and order: `RANK() OVER ( PARTITION BY cid ORDER BY grade ASC )`
			- use it with `min`: `SELECT *, min(grade) OVER (PARTITION BY cid) AS rank FROM enrolled`
		- if the window function is **aggregate** you can use `FILTER` on it like `HAVING`, usage: `MAX(numb) FILTER WHERE numb < 10 OVER(...)`, but consider always using CTE if you are confused
- common table expression 
	- like a query before your regular query
	- the output will be map to the regular query
	- usage: `WITH cteName (col1, col2) AS ( SELECT 1, 2 ) SELECT col1 + col2 FROM cteName`
	- you can use multiple CTE function: `WITH cte1 AS (), cte2 AS (), ... SELECT ..`
- another usage is **CTE Recursion**, AND try to comprehend this
	- **combining usage**: just put the `RECURSIVE`  tag at start (the first CTE function) whatever the function is 
	- it perform as
		- take the **left** expr of `UNION` as the first result of output
		- take the results to the **right** expr and use it as the new result of output
		- if the result of `WHERE` clause is NULL, the recursion will end.
	 ```sql 
	WITH RECURSIVE cteSource (counter) AS (
		(SELECT 1)
		UNION ALL
		(SELECT counter + 1 FROM cteSource
		WHERE counter < 10) /* the line of recursive statement */
	)
	SELECT * FROM cteSource;
``` 

Furthermore Reading:
	[[15445 - H01|CONSTRUCT THE SQL QUERIES]]