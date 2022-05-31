- introduction of version control
	- `git` use directed acyclic graph (有向无环图) to model history
		- use hash to represent versions commit (sha-1 `hash()` mention below)
		- use terminology to stand for the items to be stored
			- folder -> `tree`
			- file -> `blob` (binary large object)
		- the node before previous commit is immutable
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
	- `git` manipulate reference data or object data


- concrete
	- `git init` initialize git repository
	- `git help [command]` eg `git help init` view the help page
	- `git status` to status about this commit (the incoming snapshot)
	- `git commit` commit the snapshot right now
		- and it will produce a hash of that certain **commit**
		- you can print the internal metadata (eg. tree hash OR author) of commit with `git cat-file -p [commit]`, `-p` is for pretty print it
			- actually you are allowed to print a tree with `git cat-file -p [tree]`
			- also, you can print the blob `git cat-file -p [blob]`
	- `git log` show the commit history
		- `git log --all --graph --decorate` to list it as a graph
	- git stage
		- it gives a area for including the files need to be save in next snapshot
		- `git add [file]` add a file to stage
			- and also, it will add a file for **tracking**, that means, the file will be added to stage (or commit) if you use `git commit -a` in the future
				- NOTE: `git add -A` or `git add :/` will add all files included the one can be tracked and not staged for commit
				- #todo tutorial of adding for line
	- branch
		- a branch  `master` is more likely a **pointer** to a commit
			- `HEAD` where you currently looking right now
	

