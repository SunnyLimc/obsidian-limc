- Computer
	- CPU
	- 32 user registers
	- PC
	- MODE
		- supervisor -> are restricted to the following
			- Registers
				- SATP
				- STVEC
				- SEPC
				- SSCRATCH
			- USE PTEs w/o PTE_U
			- (even can not read PTE_U)
			- **You can't access physical memory directly with this mode**
	- SATP
	- STVEC
	- SEPC
	- SSCRATCH
- Process of ecall
	- `write()` ecall
	- `uservec()` -> trampoline (assembly)
	- `usertrap()` (c code)
	- `syscall()`
		- `sys_write()`
	- `usertrapret()` (c code)
	- `userret` -> trampoline (assembly)

- some hint of gdb
	- [[GDB Basics]]
	

- log form gdb need attention
	- `csrrw`
	- `csrw`

- `a` and `d` bit both is set by hardware

- lists of ecall does when it was called
	 - set mode to `supervisor` 
	 - SAVE current `pc` to register `$sepc`
	 - set `pc` to `$stvec` handler

- since xv6 doesn't provide a way to access the status of kernel or process, the states need to be saved in somewhere and at some point to be restored
	- kernel_sp
	- kernel_hartid -> kernel (CPU core) number
	- kernel_trap
	- kernel_satp

- `trampoline` is fixed, so kernel and userpage mapped to the same position and will not cause any crash during change `satp`

- RISC-V is designed to use `TRAPFRAME` since that is how the user pagetable is constructed
	- As you see, we can only determine that trapframe is available, since the system has transited to supervisor mode during the call to ecall instruction ![[Pasted image 20220508194703.png | 600]]
- use `kernel stvec` after we deep into kernel. If you have any Q, check out the last paragraph of xv6 book in §4.5 . Don't forget to explicitly enable `stvec`.

- return value is sticked to `$a0`

- compile may change register, so to save some states with C code need to do as early as you can

- prepare `satp` and `TRAPFRAME` in `usertrapret` and pass it to `trampoline` (`userret`)

- lists of `sret` (the end of `userret`) doing when calls to it
	- **re-enable interrupt**
	- replace `$pc` with `$sepc`
	- switch to user mode


Further Read:
	[[6.s081 - lab - Traps]]