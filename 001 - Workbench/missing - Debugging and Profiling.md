- set hierarchical errors that you can easily do filtering
- timestamp, color and line number
- shell escape character for applying RBG colors

- UNIX apps log file located at `/var/log`
	- use helpful parser `cat /var/log/system. log | lnav

- MAC Only
	- show sys log for last 10 seconds: `log show --last 10s`

- use `logger [str]` append any log to system log

- python module `ipdb` for debugging
	- `l` list the code
	- `s` one step in
	- `c` continue
	- `p [var | function]` print the var
	- `q` quit
	- `b [line]` set breakpoint
- `locals()` print all local variables in dictionary
- static analyze piece of code
	- `pyflakes`
	- `mypy` is another checker
	- integrate with vim

- use `tac`(the reversion of `cat`) to reverse output

- check time period with `time command &> /dev/null`
	- real time
		- in python
		- `start = time.time()`
		- `print(time.time() - start)`
	- user time
	- system time

- English grammar analyzer `writegood`

- use GDB debugging for binary
	- example:
		- `gdb --args sleep 20`
		- `run`
		- press `^c`
		- gdb will show the info of prog

- trace the used system call of prog
	- `sudo strace [-e look_specific_func] command > /dev/null`
		- `man starce` is usually helpful

- tracing profiling, to get the **bottleneck** of program
	- time
		- use a python module `python -m CProfile pyfile`
		- another human readable one `python -m kernprof -l -v pyfile`
		- rem that you need to add `@profile` for func
	- memory
		- `valgrind` for C language
		- `python -m memory_profile pyfile`
	- 