Create a new instruction "test-and-set" (ts)
- `ts reg, mem[addr]`
Atomically loads memory location into register and set contents of location to 1

```armasm
Lock:
	ts r1, mem[addr]
	bnz Lock
	// fall-through critical section
Unlock:
	st mem[addr], #0
```

Bus instruction : **Read-Modify-Write X** or RMW for short
- In writable state, hit
- In readable state, invalidate and request a writable copy

## Problem
**Excessive** coherence traffic -> Two processors invalidate a each other's line in cache when they are trying to acquire a lock that a third processor holds

![[Pasted image 20221121105027.png]]