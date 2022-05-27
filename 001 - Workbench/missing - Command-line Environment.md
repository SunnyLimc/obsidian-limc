> use `most` to get a better `$PAGER` experience

- manage processes
	- process's output will be ignored with using `nohup`, and will ignore `hangup` signal (e.g. program will still running after closing the terminal)
	- use `^Z` to suspend job, and use `bg %[jobid]` to send it with `continue` signal and they will keep running in background (you can check the list with `jobs`)
		- something very similar: `fg`, recover to foreground and reattach to your output
	- add `&` and your prompt will not be took over by program
	- send any signal to process `kill -[SIG] %[jobid]`

- 