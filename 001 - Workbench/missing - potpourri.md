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