- introduction of version control
	- `git` use directed acyclic graph to model history
		- folder -> trees
		- file -> blobs
	- the model is something like this
	```python
	type blob = array<byte>
	type tree = map<string, tree | blob>
	type commit = struct {
		paretns: array<commit>
		author: string
		message: string
		snapshot: tree
	}

	type object = blob | tree | commit
	objects = map<string, object>

	def store(object o)
		id = hash(o) # from value of object to a short hash
		objects(id) = o
	def load(id)
		return objects[id]
``