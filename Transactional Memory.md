- [[Coarse Grained Locks]]
	- Easy to code, but bad performance
- [[Fine Grained Locks]]
	- Hard to code and reasonable performance
- [[Lock-Free Datastructure]]
	- Very hard to code and best performance

## Performance Comparison
![[Pasted image 20221122054144.png|400]]

## Modifying the Hash Table
##### Hand Over hand Locking
1. Grab lock as we traverse the list
2. Do not release `cur` until we have `cur->next`

#### Deletion
Lock previous node **and then** target node

--> [[Speculative Lock]]

## Other Problems
- Priority Invesion : Higher priority thread waiting for a lock held by a lower-priority thread
- Convoying : Threads holding locks and are *de-scheduled*
- Deadlocks/Livelocks : Lock cyclic dependency or thread looping go get lock

## Idea : Declarative Locks
- Declare an intention and leave the runtime to solve it
- e.g., Forking threads in OpenMP

-> Programmer declares a variable so its updates will be atomic, language runtime figures it all out. No need for manual fences or sync. variables

-> Hardware Transactional Memory does this

## Properties
- Atomicity : Upon transaction commit, all writes *take effect* at once, on abort, none does.
- Isolation : No other processor can observe writes before *commit*.
- Serializability : *Transactions* seem to commit in a single serial order. The exact order not being guaranteed

### Transactions are composable
Transactions compose gracefully
- Programmer declares global intent
- No need to know about the implementation

## Hardware changes for TrMem
- Assume no conflicts occur
	- Record reads and writes between `xbegin` and `xend`
	- Keep all speculative state in the pipeline
- Check for conflicts in the data structure
- Other corner cases to roll back : Cache eviction for blocks touched by critical section
- On conflict, execute specified recovery code

### Recovery Mechanism
Rely on user code for recovery
```
	xbegin recover

	ld r1, mem[obj]
	addi r1, #10
	st mem[obj], r1

	xend // commit trans.

recover:
	br rec_handler
```

## Extension : Data in cache
- Simple idea : Add some bits to mark certain cache lines as speculative
- Check for conflict at every operation 
	- "I suspect conflicts might happen, so always check to see if one has occurred upon each coherence operation"
