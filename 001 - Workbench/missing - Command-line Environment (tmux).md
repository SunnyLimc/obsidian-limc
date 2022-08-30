> ~~use `most` to get a better `$PAGER` experience~~
> Actually `less` is the better

- manage processes
	- process's output will be ignored with using `nohup`, and will ignore `hangup` signal (e.g. program will still running after closing the terminal)
	- use `^Z` to suspend job, and use `bg %[jobid]` to send it with `continue` signal and they will keep running in background (you can check the list with `jobs`)
		- something very similar: `fg`, recover to foreground and reattach to your output
	- add `&` and your prompt will not be took over by program
	- send any signal to process `kill -[SIG] %[jobid]`

- Multiplex - `tmux`, keeping execution although received `hangup` signal
	- conception
		- Session
			- Windows
				- Panes
	- `^b + d` detach from session
		- `tmux a` re-attach to that session
	- `tmux new -t [name]` start a new session
		- `tmux a -t [name]` to re-attach
	- `tmux ls` to list sessions
	- window switcher
		- `^b + c` create a new window
		- `^b + p` previous tab
		- `^b + n` next window
		- `^b + [num]` to specific window
		- `^d` close a pane (if close the last pane on the last window, it will eventually close the session)
	- panes management
		- `^b + "` split window horizontally
			- `^b + %` split window vertically
		- `^b + [arrowKey]` switch between panes
		- `^b + space` rearrange panes
		- `^b + z` to zoom-in a pane, and retaped it to zoom-out

- giving an alias mapping for certain command with `alias [alias]=[command]`
	- check a alias with `alias [alias]`

- `export [env]=[value]` make environment variable available for any sub-process

- use `ln -s [filePath] [linkPath]` to create symbol link

- SSH - forward remote shell to local
	- `ssh [user]@[url/ip]`
	- `ssh [user]@[url/ip] [command]`execute a command directly instead of connect to shell, the **output** and **input** of that command can be used with **pipe**
	- `ssh-copy-id [ssh_login]` install a key to remote server and do not need to tap passphrase anymore
	- `scp [local_path] [ssh_login]:[remote_path]` copy file from local to remote path (remote relative address start with `~`)
	- `rsync . -avP [ssh_login]:[remote_path` if you want to copy a full path to remote with breakpoint retransmission support
	- configurate of SSH
		- and then you can simply use `ssh vm` to establish connection to your remote host
	```conf
	Host vm
		User jjgo
		Hostname 192.168.246.142
		identityfile ~/.ssh/id_ed25519
		RemoteForward 9999 localhost:8888
	```
	- make good use of `tmux` and `tmux a`
		- use separate key binding to control two running `tmux`, and edit the configuration to distinguish them