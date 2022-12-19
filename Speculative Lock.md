**Summary** : The critical section tries to do every change in a sneaky way without anyone noticing and if it detects a conflict it rolls back. If it rolls back too much it retries with a lock.

## Hardware Changes
Need to track all memory addresses speculatively accessed in the critical section
- For puproses of rolling back on a conflict

### [[LL-SC]] Speculative Link Register (Conflict Detection)

- Scaling the "link register" idea to memory
	- CPU pipeline already has a structure for memory speculation - called the [[LoadStore Queue]]
- Simple Mechanism : Don't let instructions retire, all addresses stay in [[LoadStore Queue|LSQ]]

- Check for conflicts in the critical section
	- Coherence indicating data race (R/W, W/R, W/W)
		- e.g. BusRd matches a ST in the LSQ (R/W)
		- e.g. BusRdX or BusInv matching a LD (W/R) or a ST (W/W) in the LSQ
- Other corner cases to roll back
	- Cache eviction for blocks touched by critical section
- If multiple rollbacks, retry with the lock

## [[Hardware Lock Elision]]

### Using Locks
- `ld LCK` returning a 1 turns off lock elision
	- Someone is in the critical section with the lock grabbed
	- Start with `ts LCK` as usual, wait for your turn
- Multiple rollbacks turn off lock elision
	- Need to make sure there is forward progress
	- Start with `ts LCK`, ts becomes RMW

### In intel Processors
- `xacquire` and `xrelease` : Defines the beginning and end of critical section
