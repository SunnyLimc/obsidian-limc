- Do NOT use `recursive lock`
- Dead Lock
	- If two lock on distinct thread was called for `lock` and `unlock` on a contrary sequence at the same time (aka. critical zone overlap), Dead Lock will occur. ![[Pasted image 20220507085718.png | 400]]
- `Spurious wakeup`
- `signal` is to notice resource is available and `broadcast` use to indicate state change
- `Condition` usually used to implement synchronize measure, like `BlockingQueue<T>` or `CountDownLatch`
	- `CountDownLatch` is useful to union child thread to wait the main thread or vice versa
- Proficient in two synchronization primitives before using other high-tech means like lock-free
- `copy on write` and `read-copy-update`
- don't use `rwlock`, 