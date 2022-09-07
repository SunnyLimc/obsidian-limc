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

## L2

- aggregates (`select`)
	- different SQL will have different performance for same result, but usually complier will do optimize to it
	- the output of a column have the values outside of aggregate is `undefined`
		- You can do `GROUP BY` to solve this ![[Pasted image 20220506213348.png]]
	- can not use a result of aggregate before calculate it
	- common aggregate functions
		- `avg(X)`
		- `count(*)`
		- `count(X)`

- some common operations
	- `LIKE` with `%` match any substring including empty AND `_` match one char
	- `||` or `+` or `CONCAT`
	- `INTO` output redirection, the structure MUST correspond to the target table 
	- handle `string` and some function like `DATE` are distinct between databases
	- `ORDER BY`, like `ORDER BY grade DESC, sid ASC`
	- `LIMIT` and `OFFSET`

- nest queries
	- complier will automatically do a optimize on it, for it don't believe to run the same thing to get same result in double for-loop
		- `SELECT name FROM student WHERE sid IN (SELECT sid FROM enrolled WHERE cid = '15-445')`
		- the same as
		- `SELECT (SELECT S. name FROM student AS S WHERE S. sid = E. sid) as sname FROM enrolled AS E where cid = '15445'
	- CMDs
		- `ALL` for all rows in sub-query
		- `ANY` or `IN` for at least one in sub-query
		- `EXISTS` if least one row matched, true will be returned.
	- a inner query can use the `REF` from the outer query
	- something interesting![[Pasted image 20220507132343.png]] `e.sid` do not correspond a column named `s.name`![[Pasted image 20220507132646.png]] use nest queries instead
	- `NOT EXISTS(<inner query>)`
- window functions
	- `SELECT ... FUNC-NAM() OVER (...) FROM table`
	- `ROW_NUMBER()` row number of current row (window)
	- `RANK()` order position of current row, NEED use with `ORDER BY` to get the correct result
	- `PARTITION BY` AND `ORDER BY`
	- Try to comprehend this :
		```sql
	SELECT * FROM (
		SELECT *,
		RANK() OVER (PARTITION BY cid
		ORDER BY grade ASC)
		AS rank
		FROM enrolled) AS ranking
	WHERE ranking.rank = 1
		```
	- `WITH ... AS ...` do a temporary calculation ![[Pasted image 20220507144249.png]]
	- one use is CTE Recursion, AND try to comprehend this
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
		- `login` the attr named 'login'
		- `*` all attributes
		- `1` plus one for each match #q
		- `DISTINCT login` only count for **unique** 'login'
	- multiple aggregation supported
- limitation for aggregation
	- use `GROUP BY` to collect extra information from table and avoid **single** confused and undefined output for the aggregation, due to no coordinate entry correspond to that aggregation. `GROUP BY` buck entity together and use those group to aggregate
- `HAVING` reference anything from your **output** list, whereas `WHERE` refer to the **input** list
- I you can peek ahead for SQL, like what DBMS needs is to **optimize query plan** and you'll get a shorter time. Like throw out the tuple not **in accordance with** the `HAVING` clause during I do `GROUP BY` or `AGGREGATE` computation. (NOT need to wait until handling the output list and do a bunch of wasted work)

- case **sensitive** string
	- expect MySQL it's case insensitive
	- use **single quote** (some of them support double)
- **string matching**
	- use `LIKE` for matching
	- `%` **any** substring and included the **empty**
	- `_` **any one** character
- string operation
	- **allow** in **output** and **predicates**, not like aggregation, it almost everywhere
	- `SUBSTRING(str, pos, len)` and `UPPER`
	- `str1 || str2` is **standard** to concatenate, and `+` in MS SQL, and `CONCAT(str1, str2)` in MySQL

- DATE/TIME
	- **allow** in **output** and **predicates**, it almost everywhere
	- `NOW()` is **standard**, `CURRENT_TIMESTAMP` is various from func to keyword
	- `EXTRACT(1 FROM 2)` extract from field calculate date EXCEPT SQLite, subtract `-` `DATE(n)` ONLY correctly work in pavlo
		- use `julianday` in SQLite and subtraction and cast it to INT. (**most used**)

- DUMP result
	- with `INTO`, you can dump your result to a **NOT already defined** table with same `#` of columns, is the **standard**
		- `SELECT DISTINCT cid INTO xx FROM xxx`
		- use `CREATE TABLE xx (SELECT DISTINCT cid FROM xxx)` in **MySQL**
	- with `insert`, you can append results to an existing table, **must** with the same column scheme
		- `INSERT INTO xx (SELECT * FROM xxx)`
		- be aware of **violate** unique primary key principle

- **OUTPUT** Control
	- **sort output** by `ORDER BY <column*> [ASC|DESC]` and the default is ASC
		- no restriction **unlike** `group by`, it will find the needed data
	- `LIMIT` the counts returned to output
		- `LIMIT 20 OFFSET 10` you can set an offset
		- if the result is **unsorted**, the result is not guaranteed the same

- nested queries
	- Most DBMS would optimize it to **`JOIN`**, you won't double LOOP, cuz inner query should be **run once**
	- You have:
		- `ALL`
		- `EXISTS`, only return **one tuple (emphasize tuple)** wherever it exists
		- `IN`, `SELECT name FROM stu WHERE sid IN ( SELECT ... )`, any tuple matching is acceptable
		- `ANY` = `IN`, the syntax is usually `WHERE sid = ANY( SELECT sid FROM xxx )`
	- allow almost **everywhere**
		- **output statements**: `SELECT ( SELECT S.name FROM stu AS S WHERE S.sid = E.sid ) AS sname FROM enrolled AS E WHERE cid = '15-445'`, can actually process the output
	- handle aggregation without `GROUP BY`
		- `ALL` Math manipulation: `SELECT sid, name FROM student WHERE sid = > ALL( SELECT sid FROM enrolled )`
		- `IN` : with aggregation `SELECT sid, name FROM student WHERE sid IN ( SELECT MAX(sid) FROM enrolled )`, and with order by, `SELECT sid, name FROM student WHERE sid IN ( SELECT sid FROM enrolled ORDERED BY sid DESC LIMIT 1) `
		- `NOT EXIST`: `SELECT * FROM course WHERE NOT EXISTS( SELECT * FROM enrolled WHERE course.cid = enrolled.cid )`

- window function