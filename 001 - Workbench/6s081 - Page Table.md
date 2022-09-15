#### Pre-questions

- What is PTE?
	- a 56-bit value mapping vaddr(index) to paddr and contains control bits(Flags)
	- 44bit + 10bit(Flags)
	- 2^27 PTEs available
	- vaddr is just a value you should not needing care of
- Where are the pagetable saved?
	- in physical memory
	- one PTE required 4096-byte to store 512 entries of PTEs (the top needs 4096-byte itself)
		- 54bit * 512 / 8 = 3456-byte - **whole 512-bit final-PTE is used for a specific block**
	- kernel to handle PTE not present that paging hardware raised #qa
- Three layers of table?
	- top(top 9-bit)-middle-final
	- memory-efficient
		- if the application memory using is unknown to you, you do not have to allocate a bunch of memory for it. #qa
		- keep memory sequence and continual for specific app. #qa
- What is TLB (Translation Look-aside Buffer) ? #q
	> To avoid the cost of loading PTEs from physical memory, a RISC-V CPU caches page table entries in a Translation Look-aside Buffer (TLB).
- CPU is using page table by reading `satp` reg. Each has one.
	- pagetable is a private address space for each CPU? #q
- Typically kernel mapping all p memory to it's pagetable
	- is it enough space to store? #q
	- or just mapping it each boot? #q
	- any conflicts to it's memory efficient model? #q
- Instructions use only virtual address.