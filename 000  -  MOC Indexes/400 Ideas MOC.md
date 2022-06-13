[[000 Home]] 

# Ideas
Questioning yourself is a philosophy way to life.

- [[Number Permutation]]

- [[Number Partition]]

- #TODO [[proof of (n times n-1 time ... 1)]]

- [[segment address]]

- #TODO 动态规划中关于相互独立和去除不确定性的说明
	- 通过状态来固定条件

- 在某种特定的限制下，选择的次序会对结果产生影响。但如果选择产生的结果是有限的，就可以通过列举所有产生的状态来解决（如果有两种限制，复杂度升次即可）。此类 pseudo np-complete 问题，一般可以用动态规划解决

- If you treat ACK is a confirmation for STATUS (eg. correctly received packet no. X), that why FIN and SYN occupy 1 seq no is intelligible
	- if you don't specify status for a updating (a unique seq no) state, It can not be confirmed.
	- If you are in a mess with ACK NO and SEQ NO, try to treat SEQ NO is the very start point of X.th status, and ACK NO is the very last point of X.th status
		- it's just a norm.
	- If you would not stuck into infinite recursion philosophy of ack, one side is needed for compromise, in other words, the last acknowledgement is uncertain for whether opposite is actually received (because you can not ACK for ack), so you need waiting until a acceptable period of time.
- random SEQ NO is a trick for preventing guessing current progress of connection, because most of HTTP connection is unencrypted and use one-time authentication. (Even so, this still does not solve man-in-the-middle attacks, but it actually did a great job for preventing IP forge)

- 为什么在计算中数码不能左移？
	- 数码的位数是固定的，左移和右移均会溢出，对于一个操作而言，右移产生的精度影响更小
- 我不移行不行捏？
	- 不对齐是无法计算的呢，两个数所对应的二进制位的区域是不一样的。在同等的数码范围内，如果不对齐，你如何确定计算范围是更大的两个数呢？(好奇脸)
	- 只能损失精度了捏
- 为什么要用变形补码，如果溢出了怎么办？
	- 变形补码是用来判断溢出的
	- 溢出则表示这个数无法在当前的储存机制下储存
		- 浮点数在计算时，可以通过左归和右归解决
		- 由于补码的 1/2 机制，溢出是不明显的且会造成错误解，所以需要更多的判断机制
			- 比如变形补码
- 那么，变形补码是如何检测溢出的呢？
	- 