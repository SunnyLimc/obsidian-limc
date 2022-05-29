- introduction of version control
	- `git` use directed acyclic graph to model history
		- folder -> trees
		- file -> blobs
	- the model is something like this
	```go
	type blob = array<byte> (binary file)
	type tree = map<String, tree | blob> (a folder mapping)
	type commit = struct {
		paretns: array<commit>
		author: string
		message: string
		snapshot: tree
	}
```
	- jkkkkkkkkkkkkk