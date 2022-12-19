- Specify a minimal "memory operation"
	- Indivisibly accesses a certain address (not as two separate memory operations)
- Two operations conflict if they access the same location and at least one is a write
- A data race is when :
	- Two conflicting operations from different threads occur simultaneously
	- Simultaneous operations are defined as back-to-back accesses in any sequentially consistent interleaving

(When one thread is faster than the other or slower, does it change values ?)

## Checking using Happens-Before relationship
- Action in **same thread** $\rightarrow$ each other in **program order**
- Unlocking a `monitor` $\rightarrow$ all locking operations
- Writing to a `volatile` $\rightarrow$ all read of that fields
- Alls actions in a thread $\rightarrow$ a `join()` on that thread
- Transitivity: $A_x \rightarrow A_y, A_y \rightarrow A_z \implies A_x \rightarrow A_z$

- If $\{\exists (A_x, A_y) \mid A_x \text{ does not } \rightarrow A_y \land A_y \text{ does not } \rightarrow A_x\}$ then we have a data race