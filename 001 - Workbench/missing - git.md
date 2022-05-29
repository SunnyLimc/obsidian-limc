- introduction of version control
	- `git` use directed acyclic graph to model history
		- folder -> trees
		- file -> blobs
	```go
	type blob = array<byte>
	type tree = map<String, tree | blob>
	type commit = struct {
		paretns: array<commit>
		author: string
		message: string
		snapshot: tree
	}
```