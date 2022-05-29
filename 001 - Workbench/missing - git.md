- introduction of version control
	- `git` use directed acyclic graph to model history
		- use terminology to stand for the items to be stored
		- folder -> `tree`
		- file -> `blob` (binary large object)
	- the model is something mentioned above
		- surface layer
      ```python 
	type blob = array<byte>
	type tree = map<string, tree OR blob> # refer to blob / tree
	type commit = struct {
		paretns: array<commit> # refer to commit
		author: string
		message: string
		snapshot: tree # refer to tree
	}
# keep in mind: if you refer to blob, it actually refer to it's id in stroage rather than the raw
# it retrieve raw file by a load() function (will be mention below) with id
```
		- underlayer storage
		```python
	type object = blob OR tree OR commit
	objects = map<string, object>

	def store(object o)
		id = hash(o) # from value of object to a short hash
		objects(id) = o
	def load(id)
		return objects[id]
```
		- the connection is, when a `blob`, `tree` and `commit` store into storage, it actually use sha-1 **id** to identify them
		- then surface can simply save **id**, and retrieve the raw file through `load()`
		- it's worth to know that sha-1 is consist of 40-hexadecimal
	- Hashing is inconvenient for human presentation system
		```python
	references = map<string, string> # mapping form hash to readable syntax 
```