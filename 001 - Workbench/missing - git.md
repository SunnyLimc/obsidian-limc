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
	- `git stage`
		- it gives a area for including the files need to be save in next snapshot
		- `git add [file]` add a file to stage
			- and also, it will add a file for **tracking**, that means, the file will be added to stage (or commit) if you use `git commit -a` in the future
				- NOTE: `git add -A` or `git add :/` will add all files included the one can be tracked and not staged for commit
			- just add a pieces(part) of file: `git add -p ...`
	- `git branch`
		- a branch  `master` is more likely a **pointer** to a commit
		- `HEAD` where you currently looking right now
	- `git checkout`
		- it move `HEAD` and change your working directory contents correspond to that commit
		- `git checkout [file]` discard the changes made in that file of current working directory and restore it to what `HEAD` is
	- `git diff`
		- `git diff --cached`: use **staged files** instead of working directory as the default `from-commit`
		- diff commit: `git diff [from-commit] [to-commit]`
		- diff current **working directory** with others: `git diff [from-commit]`
			- you can specify file that you want to diff: `git diff [from-commit] [file]`
	- `git branch` will create a new branch point to `HEAD`'s commit
		- you can use `git checkout [branch]` make `HEAD` points to that branch
		- to list branches `git branch -vv` and it will give you the upstream branch
	- `git merge` to combine two commit
		- if this branch is located at the commit which the predecessor of the branch to be merged, it can directly `Fast-Forward` without creating any snapshots and just move branch pointer to that commit
		- `git merge --abort` abort merge and go back to the state that merge not happen
		- if you encounter with conflicts
			- `git mergetool` if you configure `mergetool`, you can finish merge with this command
			- fix them manually
				- `<< [to ours branch]` and `>> [from merge branch]`, `==` the separator of conflicts
				- fix the conflicts, and remove the identifiers
		- `git merge --continue` will continue that process
	- `git remote` to list all remotes
		- `git remote add [remote] [path]` add a remote
			- create a path for remote locally: `git init --bare`
		- `git push [remote] [branch]:[remote-branch]` push to remote, it will be created if the remote branch not exist,
		- set default remote and remote branch for `git push` of a particular local branch
			- `git branch --set-upstream-to=[remote]/[branch]`
		- references of remote that points to a particular commit will be list in `git log`
	- `git clone [link] [folder-name]` save a copy of repository form remote
		- clone but avoid to include whole history of that remote repository `git clone --shallow ...`
	- `git fetch [remote]` retrieve all changes of remote repository to local
		- `git pull` = `git fetch` + `git merge` fetch and merge branch from remote branch
		- `git merge` make your local branch points to the same place as remote branch
	- `git blame [file]` figured out how the files has been modified in history and find out who made it
	- `git show [commit]` show details of a commit, instead of using `git checkout`
	- `git stash` save current changes to stash and revert working directory to `HEAD`
		- `git stash pop` retrieve from stash
	- `git bisect` powerful tool for finding particular things from history

- file
	- `.gitignore`
		- add a file, path or **regex pattern** once a line if you don't want git to notice you to trace the file, just ignore it
		- if you are already add something to be tracked, you need to remove them from the cache first `git rm --cached [file]`