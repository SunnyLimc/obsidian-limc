### from lecture

- key remapping
	- long / short press

- daemon
	- running in background
	- systemd for Linux checking other daemon, boot startups
		- configuration

- crond (crontab)

- FUSE file system lies in Kernel level
	- can interact with user operation such as `touch`, the operation will be captured by FUSE in the kernel, and forwarded from kernel space back to user function and determine the next operation by user (like sending emails).
	- SSHFS - forward operation by SSH to remote system
	- Encrypt file

- backup
	- cloud file provider backup

- API
	- interact with online content service provider
	- fetch URL by `curl`
	- use `jq` parse JSON

- shell program parameter
	- `--help` for helping
	- `--version` for showing the version
	- `--verbose` `-vvvv (increased times of v)` for more debugging output of the prog
	- `--quiet` for less output
	- `--dryrun` trigger a mock run without modifying any actual files
	- `-i` interactive mode (more prompt decision)
	- `-r` recursive flag
	- ` - ` single standard dash, refer to standard input or output
	- ` -- ` singular double dash, everything follow this should not be interpreted
		- `rm -- -i` remove the file named `-i`

- window manager
	- float window manager
	- tailing window manager

- VPN
	- trust whom?
	- public network
	- DOH or DOT

- Markdown
	- `* *` emphasis
	- `** **` strongly emphasis

- HummerSpoon on mac for automatically doing something

- Live USB system

- Virtual Machine
	- docker for securely development and exploring for a preconfigured container

- notebook programming environment
	- run snippet of prog with interactive GUI and retrieve the result

- git contribution
	- fork repository
	- modify something
	- pull request

### from QA

- `./script` vs `source ./script`
	- invoke use a separate new shell session
	- `source` execute command with current shell session

- path
	- conventions
		- `/user/local/bin` user compiled programs
		- `/usr/bin` user program
		- `/usr/sbin` for sudo-user only
		- `/bin` essential system utils
	- `/lib` libraries that progs linked to
	- `/opt` third-party software installation (usually with the software that ported to Linux)
	- `/tmp` temporary files and clear every boot
	- `/etc` configurations
	- `/var` files will be changed over time (like .lock files package management, process IDs)
	- `/dev` for devices
	- `/sys` 

- pip vs apt-get
	- apt one-place management with apt-get but some out-of-date packages
	- pip is usually up-to-date
	- avoid compile from scratch
	- multi-version control of python packets with `virtualenv`

- profiling programming
	- print stuff with `time` to increase program performance
	- Valgrind - cache-grind
	- Flamegraphs
	- Check the minimum amount of time - time that depend on external operation
	- **BPF** or EBPF kernel time tracing

- extension
	- ublock origin
		- network filtering
	- stylus
	- multi account container

- data wrapping
	- perl
		- processing text
	- python, pandas (for table)
	- `column -t`
	- `grep`, `sed` etc.
	- vim record
	- `pandoc` conversion documents
	- statistic and plotting with R lang
	- `ggplot2`

- Docker vs VM
	- the shared kernel mechanism saved your memory (low over-head)
	- storage persistence

- **VIM**
	- leader key
	- **macros**
	- use `mX` (X stand for [A-Za-z]) to mark page
		- [a-z] is a particular identifier for **the specific file**
		- [A-Z] is a global set, allowed to be used everywhere
		- `'X` jump to there marked place
		- for more visit [Using Marks](https://vim.fandom.com/wiki/Using_marks)
	- `ctrl+o` go to previous place
	- `ctrl+i` go forward
	- `:earlier` rescue you from overwrite undo history and can not **redo**
		- `undotree` plugin
		- persistent undo history
		- `auto-save`
		- and finally, disable `swapfile`
	- registers
		- OS-keyboard

- 