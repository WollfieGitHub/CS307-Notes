# Bit Vector Directory Entries

Before we stored all of the L1 cache tags

- Instead, store a bit vector (1 bit per core) with each tag
	- If a bit is 1, then that core shares this cache block
![[Pasted image 20221011072109.png]]
