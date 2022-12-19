## aka (HLE)
Why change `TS` into `LD` ?
-> Sending `TS` means processor gets exclusive access, sending `ld` keeps the lock free

### Benefits
- Conservative Locking
	- Easier to show correctness than fine grained locking
	- Our code locks more often than necessary because the lock is on the whole hash bucket
- Locking granularity
	- Tradeoff between Perfomance and Complexity
- Thread-unsafe legacy libraries
	- Require global locking

### When does it fail ?
- Atomicity Violation : Detected by coherence protocol, restarts from `ts LCK`
- Resource constraints : Critical section bigger than ROB, LD/TS in critical section conflict in cache
- Interrupts


![[Pasted image 20221122070412.png]]

## Walkthrough
![[Pasted image 20221122121321.png]]
![[Pasted image 20221122121821.png]]
## With Cache Misses
![[Pasted image 20221122121838.png]]
![[Pasted image 20221122121851.png]]
## With Rollback
![[Pasted image 20221122122146.png]]
![[Pasted image 20221122122152.png]]
![[Pasted image 20221122122202.png]]