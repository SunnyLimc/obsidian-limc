## missing - shell

### Practices

>  should be escaped from regex:
> 	 inside cc\[ ]: `. ^ $ * + ? ( ) [ { \ |`
> 	 outside: `^ - ] \`
	
- enclose `cd` with parenthesis `()`
- `pushd` and `popd`
- always use `${var}`
- using as **Library are not mentioned**
- always be functional, use `main` as the entry
- use `""` as preferred
- no global vars, or use `readonly` on constant
- use `local` within function (readable)
- lowercase variable unless it **export** to env
- set `set -euo pipefail`, fail fast when struggle and get exit code
	- `-e` stand for stop script when get a non-zero exit code
	- `-u` stand for detecting undefined variables and stop the script
	- `-E` lead bash trap ERR inherit to its shell function (i.e. make `pipefail` aware in error msg of the shell)
- use `||` to start prog intentionally let exit non-zero
- no deprecate style. Always use `func() {}` and `[[]]` and `$(...)`
- use `./` and leverage `$PWD`
- use declare before using function like `declare arg1="$1" arg2="$2"` when function are more than 2 lines
- use `mktemp` to create tmp files and use `trap` to ensure it will be removed when script exit

	```bash
trap 'rm -rf -- "$MYTMPDIR"' EXIT
```
- warns and errors should be direct to `stderr`
- **always** check syntax with `bash -n`
- use `echo` and `$(func)` as return or use `$?` to refer that (exit or return from function both left script itself still alive)

### Tricks

- `${var-}` append a dash at variable ending can bypass **`set -u`, the unset variable aborting**

### String concatenation

```bash
# use
string3+=${str1}
# or
string4=${str1}${str2}
```

### for loop

```bash
for arg in "$@"
do
done
```

### if else

use `then` in **newline** is extremely important

```bash
if [ ${n} -eq 101]
then
elif [${n} -eq 102]
then
else
fi
```

### case

```bash
case "${n}" in
	\+86*) c=china ;;
	\+1*) c=us ;;
esac
```

### echo

use `\` instead of quote can continue enter as a same parameter

```bash
echo Hello\ World
# equal to
echo "Hello World"
```

---

`$PATH` sperate by colon `:`, the path will be searched in order

```bash
echo $PATH
  /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib
which vim                                                                                           
  /usr/bin/vim
```


###  Working Directory

* rmdir → remove empty dir, use it to keep a good habit
* mkdir → `space` to create multi dir, or use excape `\` or quotes to make dirname include space

```bash
pwd
# Print working directory

cd
# ~/ for home
# . for current directory
# .. for parent directory
# - for prevously directory

ls -l
  -rwxr-xr-x 1  limc limc  1070 Nov  1 10:06 proxy.sh
  drwxr-xr-x 7  limc limc  4096 Mar 21 09:25 work
# -rwxr-xr-x 12 limc limc  4096 Jul  6 18:59 .oh-my-zsh
# d means dir, l means link, - means file
# rwx means read, write, execute
# first column rwx means user privilege
# second column means group privilege
# third cloumn means everyone else privilege
```

### Permission

- User can choose group what they are
- only `root` user can change the owner of file
![[mZ6qv_8m20g8YPUC.png]]
- if you have a write privilege to a file but not to the directory, you **can't** remove the file but can empty the file. In opposite, you can remove it but can not write it (vim `:wq!` use this trick).
* if you want to exec a file, you need to get all its parent dir and parent parent or etc. **dir execute permission**.
- Otherwise, you won't even be able to access or remove the directory. (You can only list just subfolders in the directory).

### Helps for Usage Syntax

```bash
man PAGE...
```

`...` → zero or one or more

`[]` → optional

### Shortcuts

`ctrl + L`  → clear the console and go back to top

### Stream

rewrite the input of the program with `< file`

or rewrite the output of the program with `> file`

```bash
cat < hello.txt > hello2.txt
# rewrite cat input file from hello.txt, and write file to hello2.txt
```

`< input` → rewrite input stream **from a file**

`> output` → rewrite output stream **to a file** and **replace** it

`>> output` → rewrite output stream **to a file** and **append** it

```bash
ls -l / | tail -n1 > ls.txt
# write the last line of ls -l output to ls.txt
echo "root privilege" | sudo tee root.txt
# take the input from input stream and write it to the file and your screen
```

`input | output` → this named stream. It take **left output result** to the **right as input**, and vice versa. Most programs in pipeline are run simultaneously

> stream/input/output can apply to binary image, videos, chrome-cast too

the program that actually write/read from/to stream is **the shell itself**, so `sudo` doesn't help this at all, and you may get `permission denied`, because the shell is not the program which `sudo` execute.

#### from a temporary file 

In bash-like shell, you can use `<<<` or `<()` to create a temporary file, and redirect the content to another program

usage:

```bash
cat <<< this is content
  this is content
cat < <(another content)
  another content
```

### sysFS

`/sys` store the parameters of the running system **kernel** and you can edit it, it will take effect immediately.

> You may need to be a root user before modify it. You will get `#` when you enter a root user, which `$` indicate the user is non-root.

you can also use `tee` to achieve it.

```bash
# tee forwared input to it's output, and simultaneously print it to stdout(screen).
echo 1060 | sudo tee brightness
```

### Variables and quotes

```bash
# Right
foo=bar
echo $foo
# Wrong (space)
foo = bar # equal to invoke foo with parameter = and bar

# " == '
echo "World"
echo 'World'
echo "Value is $foo"
  Value is bar
echo 'Value is $foo'
  Value is $foo 

```

function and `source` to shell

`$0` refer to the name of the script, `$1...$9` also refer to the first through the ninth arg

`$@` get **all** the arguments (usually used in a `for-in` loop)

`$?` get the error code from previous command.

`$_` will get the last arg of previous command.

`$#` get the count of args

`$$` get the pid of the process

> the **preserved arg**s mentioned before are also can be applied to shell

```bash
# mcd.sh
mcd () {
  mkdir -p "$1"
  cd "$1" # $1 refer to the first arg pass from shell
} # $2... also refer to the second through the ninth arg
# $0 refer to the name of the script

# shell
source mcd.sh # load mcd.sh to shell (run script locally in the shell)
mcd hello # the "hello" folder will be created and shell move into it

```

### Misc

`!!` for previous command

### Error code

function return `0` for no error. `1` for errors happened.

Logical expression;

```bash
true
echo $?
  0

false
echo $?
  1
```

```bash
false || echo "Oops fail"
# the "short-circuited" algorithm like the programming language
# if the front is ture, the back will not be exectued
```

```bash
true && echo "Success"

false ; echo "This will always print"
# you can use semicolon to concat the commands in a same line
```

### Calculate expression

use `$()` to execute a command rather than serialize it

```bash
# you can storage a result from command to variable
dr=$(pwd)
echo $dr
  /home/limc
```

use `<()` to save the execute result **list**(line by line) to a temporary file, to solve the problem that `|` can not solve

```bash
# concat ls and ls.. output
diff <(ls dir1) <(ls dir2)
```

### Redirect another stream

`2` is for stderr

```bash
# if we only care about the return value, we can redirect stdout and stderr to null
grep foobar "$file" > /dev/null 2> /dev/null

# redirect output STDOUT to STDERR
cat out.txt || grep "ERROR" 1>&2 
```

**use `&` instead of a single number to reference IO, since it's not a particular file**

### test

run `test` in script

> there some differences between single square quote and double

```bash
for file in "$@" do
  grep foobar "$file" > /dev/null 2> /dev/null
  if [[$? -eq 0]]; then # [[$? -eq 0]] the same as $(test $? -eq 0)
    echo "successful"
  fi
done
```

### regex match

we can use `?` or `*` or `{...}` **etc.** to do character match.

you can also combine some of them to get a powerful shell.

> you can press `TAB` to expand the command needed to exec

```bash
ls 1.*
  1.sh 1.bash
cat 1.*
  /15 # from 1.sh
  /22 # from 1.bash
cat 1.??
  /15 # from 1.sh
cat 1.{,sh,bash} # leave comma itself empty to point to just empty (1.)
cat: 1.: No such file or directory
/15
/22
```

and bash has it's own regex-matching operator `=~`

```bash
set -- '12-34-5678' # set $1 to sample value

kREGEX_DATE='^[0-9]{2}[-/][0-9]{2}[-/][0-9]{4}$' # note use of [0-9] to avoid \d
[[ $1 =~ $kREGEX_DATE ]]
echo $? # 0 with the sample value, i.e., a successful match
```

use `${BASH_REMATCH[i]` to retrieve the result

### Set script executor 

put it at the beginning of the file, so that the shell can identify it.

```bash
#!/usr/bin/python
### or find it from env PATH ###
#!/usr/bin/env python

import sys
for arg in reversed(sys.argv[1:]):
  print(arg)
```

```bash
./script.sh 1 2 3
  3
  2
  1

```

### Tool for check script

you can use `shellcheck` to check the grammar and implicit problem of script.

### Locally vs not locally

if you run script in bash `#!/usr/bin/bash`, the code will be executed in a new process, and all the environment changes inside it will never affect the shell.

or you need to use `source` to run the script locally.

### Miscs

- A bloated `man` alternative

- you can use `tldr` to get brief example for programs

- Search you file with `find`;

```bash
tldr find
    # - Run a command for each file (use {} within the command to access the filename):
    # find {{root_path}} -name '{{*.ext}}' -exec {{wc -l {} }}\;
find work/ -name '*.cc' -exec rm -l {} \;

```

- use `fd` for regex match (alternative)

```bash
# the package packet named fd-find
fdfind 'tcp(.*).cc' --exec ls -l
```

- search all path in the system by `locate`

```bash
# you need to exec 'sudo updatedb' first
locate libsponge
```

- use `grep` or `rg` to do search inside a file

```bash
# recursive search the file insider the path
grep -R include work/
  ./work/TCP-Lab/apps/lab7.cc:#include "util.hh"
  ./work/TCP-Lab/apps/lab7.cc:#include <cstdlib>
  ./work/TCP-Lab/apps/lab7.cc:#include <iostream>
  ./work/TCP-Lab/apps/lab7.cc:#include <thread>
  ...

```

- use `rg` for faster search

```bash
# show the 5 lines around the result
rg "#include" -t md work/ -C 5

```

- Fancy matching with `fzf`

	 It will scan the whole output and match what you type, and show all of the line matched **in a screen**.

- shell `history`

	use `history` to show all the command previously exec in the shell

- press `CTRL + R` in shell can go through the history commands fast, press `CTRL + R` **again** to search next, and press `ENTER` to exec it.

- list path in a `tree` view

- use `tree` to get the tree list of current path

- use `broot` to walk through paths in a tree view
	also `nnn` can do this well