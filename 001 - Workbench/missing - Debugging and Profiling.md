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

- use GDB debugging for binary
	- example:
		- `gdb --args sleep 20`
		- `run`
		- press `^c`
		- gdb will show the info of prog