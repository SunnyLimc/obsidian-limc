- compile code with `gdb`
	```shell
gcc -Wall -ggdb -o <program> <source>
```
	- `Wall` enable all `warnings`
	- `ggdb` the most expressive format for `gdb`
	- `ggdb3` if information for macro is needed

- run with gdb
	```shell
gdb --args program <...args>
# or
gdb program

(gdb) r
#or
(gdb) run
```

- list function
	- `list <line>` or `list <fn_name>`
 
- breakpoint: `b <line>`
	- delete: `delete <bp>`, or `d <bp>` 
	- also you can set with a function name: `b LinkedList<int>::remove`
	- set **when** condition `condition <bp, eg="1"> value==1`
		- or set it directly `b <...> if <condition>`
	- set a relative line `b -/+<line>`
	- break for given function `b <file>:<fn_name>`
	- break memory address `b *(<address>)`
	- list breakpoints: `info breakpoints`

- command after a breakpoint was hit
	```shell
	(gdb) command <break_num>
	> print port
	> print IPAddr
	> print User
	> print Pwd
	> end
	(gdb)
```

- back trace the  with `bt`
	- `info stack`
	- `backtrace`

- print line of current stack with `where`

- print current line with `frame`

- `finish` or `fin` run until reach the end of `fn`

- `^L` to clean display

- python interpreter
	- `python`
	- use `gdb` package
		- e.g. `print gdb.greakpoints()[0].location`

- print variable with `p`
	- print register in **hex** `print/x $satp`
		- `print/x $stvec` -> where kernel exec the trap
		- `print $pc`
		- `print/x $sepc`
	- print the whole array simply with `p arr`
		- print just a certain range `p *&arr[<index>]@<nums>`
			e. g. `p *&[96]@5`

- add a watchpoint to notify that something been modified 
	- `watch <variable>`

- `step` step into function or next line via source code
	- `stepi` same as above but via machine code
	- or `s` and `si` is the same
- `next` is similar to `step` but not step-in function
	- `nexti` similar reasoning
	- or `n` and `ni` is the same

- ignore a breakpoint multiple times `ignore <bp> <times>`

- `tui enable` to get a tui window or `^x + a`
	- `^x + 2` twice to get (switch between) multiple window
	- `tui reg float` to get values of registers table displayed in float
	- `^p` and `^n` to get previous command if tui is enabled
		- maybe simply map key?

- reversible debug
	- `reverse-continue`
	- example (a occasionally unknown issue)
		- the default binary code that complier output is `a.out`
		- you can interpret it with a shell call `./a.out`
			- `while ./a.out; do echo OK; done`
			- sometimes you can get a core dump which `gdb -c` may able to debug it
		- a better way to achieve that is thought reversible-debug with directly `gdb a.out`
		- set breakpoint on `main` with `b main`, also set a bp on `exit` with **`b _exit()`**
		- use `command` to find out the error 
			- `command <main_bp>` -> **`record`** `c` `end`
			- `command <exit_bp>` -> `run` `end`
		- the program will keep running repeated until get errors
		- use `b` and **`watch`** to disclose more
			- use `reverse-continue` to reverse run until reach the breakpoint or **the line will modify certain watched variable**
	- may add `-static` option will do helps with debugging

- use `x` to examine memory
	- `x/..` eg `x/2c $a1`
		- `x/6i 0x3ffffff000` -> `TRAML`
		- `x/4i $t0`

- quit with `q`

- `info reg`

- `^A + ^C` enter qemu
- `paint variable`	
- `info mem`
	- the last two of list is `TRAPF` and `TRAML`
- `print/x %scause`

- `run` run program until hit a breakpoint



- `up`
	- `down`