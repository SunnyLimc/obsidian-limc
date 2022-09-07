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