[[000 Home]] 

# Ideas

Questioning yourself is a philosophy way to life.

### An immediately impulsive action for changing myself

- do not procrastinate
- listen to the class carefully

### To-Do List

- [x] fix direction hotkeys can not be combining used with other modifier keys.
- [x] build K30Pro for a new PE version to fix GMS issue
	- [x] FAIL: boot stuck. Workaround: back to MIUI
- [x] fix dotfiles synchronization between Cygwin and WSL
	- [x] fix `nvim` popup, suppress Enter key to confirm a autocomplete
		- [x] fixed, it has hidden in snippets/vim-cmp extra lua script
- [x] synchronize SSH key between WSL and Windows
- [ ] Use Arch Linux as my default distribution
	- [ ] fork repo and edit it
- [ ] make `less` colorful
- [ ] try to figured out how the OCR program works and convert it to AHK.
- [ ] improve NGINX reverse docker proxy for a future use

### Prefect idea candidates

- A GUI tool for Autohotkey like Quicker
	- electron
		- for the most backward windows compatibility
	- react
		- framework

### temporary

[[temp]]

### Helpful practices 

- sign all your commit 

````bash
git filter-branch --commit-filter 'if [ "$GIT_COMMITTER_EMAIL" = "limc@limcode.cn" ];
then git commit-tree -S "$@";
else git commit-tree "$@";
fi' HEAD
````

- find the final URL by curl

```bash
curl -Ls -o /dev/null -w %{url_effective} http://google.com
```

### Draft

Assuming that we have $\left(\begin{matrix} k_1 \\ k_2 \\ \vdots \\ k_n \end{matrix}\right)$ should follow the transformation **T**

Instead of telling you what **T** is. I can just describe the result of $T(e_i)$, which $e_i$ is the $i_{th}$ column of the *E matrix*, contains $0s$ in each index whereas the $i_{th}$ has a $1$, for example $e_1$ is alike $\left(\begin{matrix} 1_1 \\ 0_2 \\ \vdots \\ 0_n \end{matrix}\right)$, and the $T(e_i)$ is alike $\left(\begin{matrix} a_{1i} \\ a_{2i} \\ \vdots \\ a_{ni} \end{matrix}\right)$, just a transformation of $e_i$

if you treat $\left(\begin{matrix}k_1 \\ k_2 \\ \vdots \\ k_n\end{matrix} )$


and create a vector $\left(a_{1i}, \cdots\right)$, and $\left(e_{1,}\cdots \right)$ is the scalar of $k_i$ in this transition, if we take $\left(k_1, \cdots \right)$ into this transform **T**, instead of know what the transform is, we just need to know what $\left(e_1, \cdots \right)$ as scalar vectors, so the multiply $k_1$ by $\left(e_{1}, \cdots\right)$ and calculate the sum

we should only keep track of the matrix {a_11, a_12, .., a1n} (is described in below as a_in) to describe the transform

in this way, we can concrete T into a specific language, assume there is a vector we want to transform {x_1, x_2, ... x_n}^t (the transpose vector), we just need taking a composition of {a_in} and {x_1, ...}^t

we describe the transformation process T as $R^n$ -> $R^m$ with a $m \times n$ matrix

#### So, why mathematician write down as n -> m?

Every time we describe the $m \times n$ matrix as a transformation T.

It basically a process take **n-row as input** and with doing the *m-row transformation* (with the *nth-column transformation* corresponding to *nth-row*) and producing a **m-row output**.

If I am pushed to say, it's because the *input matrix* laid at the right whereas the *output matrix* laid at the left.

$$\left(\begin{array}{cccc}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
 \vdots & \vdots & \ddots & \vdots \\
 a_{m1} & a_{m2} & \cdots & a_{mn} 
\end{array}\right)
\left(\begin{array}{cccc}x_{1} \\x_{2}  \\\vdots \\ x_{n}\end{array}\right)
=
\left(\begin{array}{c}{{a_{11}x_{1}+a_{12}x_{2}+\cdot\cdot\cdot+a_{1n}x_{n}}}\\ {{a_{21}x_{1}+a_{22}x_{2}+\cdot\cdot\cdot+a_{2n}x_{n}}}\\ {{\vdots}}\\ {{a_{m1}x_{1}+a_{m2}x_{2}+\cdot\cdot\cdot+a_{m n}x_{n}}}\end{array}\right)$$
So, what about the composition?

If define $T\colon\mathbb{R}^{n}\to\mathbb{R}^{m}$ and $S\colon\mathbb{R}^{p}\to\mathbb{R}^{n}$, the combination is $T\circ S$ ($S\colon\mathbb{R}^{p}\to\mathbb{R}^{m}$), which calculate the same as above from *the right to the left*

we will get the *composed linear transformation* matrix
$$\left(\begin{array}{l l l l}{b_{11}}&{b_{12}}&{\cdot\cdot}&{b_{1p}}\\ {b_{21}}&{b_{22}}&{\cdot\cdot}&{b_{2p}}\\ {\vdots}&{\vdots}&{\cdot\cdot}&{\vdots}\\ {b_{m1}}&{b_{m2}}&{\cdot\cdot}&{b_{m p}}\end{array}\right)$$



### Thoughts

> Things can not be archived in a specific subject

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
	- 不对齐是无法计算的呢，两个数所对应的二进制位的区域是不一样的。在同等的数码范围内，如果不对齐，你如何确定计算范围是更大的两个数呢？
	- 只能损失精度了捏
- 为什么补码的方式可行？
	- 计算机里，二进制的能表示的位数一般是固定的。我们的目标，是找到一种关联，将正数与负数连接起来。One's Complement 和 Two's Complement，或者很多奇奇怪怪的 offset binary，都是一种关联的办法
	- 找到关联了还不够，由于二进制的表达形式，我们希望在尽可能保留二进制含义的基础上（也就是原数的二进制结构），关联他们的算术运算
	- 由于补码并不会产生重复的数，而且也能够自然地关联到算术运算上，所以就被采用了
		- 它能够关联到算术运算，本质上是依靠偏移量来实现的。这里的偏移量，指的是与 0 的偏移量。
			- 由于位数固定的特性，当运算结果溢出时，高位的数据会被抛弃。此时 $上界 + 1$ 和 $0$ 在本质上都是一样的，偏移量都为 0
				- 这样一来，二进制数就被抽象成了一个数轴
			- 此时机智的你，一定会把正数和负数，均等地放在中位数的两边，来表示尽可能多的正负数
				- 由于二进制的特性，就使用了最高位作为符号位来分开正负数
				- **但**，如果操作时偏移量从数轴的末端溜走了（补码的数轴末端分别为 $1000\dots$ 和 $0111\dots$）就会造成下溢或上溢，也就是**下文谈到的溢出**
	- EOF
- 如何判断补码是否产生溢出？
	- **核心就是正数加正数不能得到负数，负数加负数不能得到正数。其余随便，反正也溢出不了**
	- 对于前者，只需判断两者符号位是否都等于 0，运算之后结果符号位是否为 1
	- 对于后者，是一样的。
	- 实际计算中。对于正数加法而言，不应该出现进位。对于负数加法而言，如果 MSB 上出现进位，那么第二有效位也应该要出现进位，以便得到一个负数。对于正数加负数的情况，第二有效位的进位跟 MSB 的进位是保持一致的。
	- 还有一种方法，即在符号位的前面多引入一个**等值**的二进制位来记录符号位的进位情况。如果有进位从第二有效位传到 MSB，那么 MSB 也会产生一个进位作用到这个额外的符号位。如果**只有 MSB 的进位传递到额外的符号位，而没有第二有效位的进位，那么就能判断是溢出**
	- 总结以上的规律就是，如果 MSB 上出现进位，那么第二有效位也应该要出现进位
	- *上文的 符号位 和 MSB 是同一个东西
- 依照上文的知识点，你可以很好地理解一种，新的有符号数表示法
	- 以下也是可行情况
		- $0000$ -> 0
		- $[1000,1111]$ -> $[8, 1]$
		- $[0001, 0111]$ -> $[-1, -7]$
	- 那何时溢出？
		- 由于切割点跟补码一样，所以溢出的判断情况跟补码一致
- 为什么要用变形补码，如果溢出了怎么办？
	- 变形补码是用来判断溢出的
	- 溢出则表示这个数无法在当前的储存机制下储存
		- 浮点数在计算时，可以通过左归和右归解决
		- 补码溢出会造成错误解，所以需要更多的判断机制