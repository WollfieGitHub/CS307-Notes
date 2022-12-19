For this I really recommend my future self to take a loot at the video :
https://tube.switch.ch/videos/06P5qGSJDv
## Note
- Modern processors are :
	- Superscalar
	- Out of order
- Caches are :
	- Non-blocking, multi-ported
	- Buffered at input/output

- A multiprocessor behaves like a "multitasked" single core
- Memory appears like it has a "switch"
![[Pasted image 20221019154304.png|100]]

More formally :
- "The result of any execution is the same as if the operations of all processors were executed in some sequential order, and some individual processor appears in the order specified by its program" 

## Requirements
1. Memory accesses happen in program order
2. Memory operations *appear* to be atomic (Instantaneous to other processors in the system)

### Reminder : OoO CPU Pipeline
![[Pasted image 20221019154629.png|200]]
-> From Comp Arch II : [[Reorder Buffer]]

### Ordering Memory Instructions
We can't know memory addresses until they resolve
- Track the FIFO Program order of loads & store
- Resolve addresses *when they are ready*
- On a **load**, check for the **yougest** store to this address

-> Use a structure called a "[[Load Store Queue]]" (LSQ)

## [[Load Store Queue]]


## Performance
Memory consistency models affect performance so we need to dictate which memory re-orderings are acceptable

#### Specifications
Recall three types of memory dependencies :
from [[Cache Coherence]] : RAW, WAR, WAW

- Given $Store_i(A, V) << Load_j(A)$ ($<<$ means "precedes")
	- $Load_j(A)$ must return a $V$ if there isn't a $Store_k(..)$ somewhere such that :
	- $Store_i(A) << Store_k(A, V') << Load_j(A)$
- We can then guarantee the latter by using these dependencies :
	- RAW : $Store(A, V) -> Load(A)$
	- RAW : $Store(A, V) -> Store(A, V')$
	- RAW : $Load(A) -> Store(A, V')$

### Why wait for stores ?
- As `Store`s do not generate operands for the core (only arithmetic and loads do), processor should continue while store is pending
- -> Reclaim LSQ entry for new memory operations

#### Solution : Add [[Store Buffer]]
-> Reduce scarcity in LSQ to use it as efficiently as possible. Stores don't generate anything for the pipeline, only for the memory. So we put them in the buffer and are **considered as part of L1** and cannot be undone.They are executed whenever we have time.
-> Now loads must check SB as well as L1 because there is no guarantee on when values will be written

- Maintain program order in the CPU
- **Must** maintain atomicity in the memory system
	- In small-scale systems, use a shared bus
	- At larger scale, explicit completion ACKs

#### Problem with the [[Store Buffer]]
A true "Sequentially Consistent" multiprocessor would be really slow because it can only issue one memory operation at a time.

##### [[StoreBufferExamplePerformance|Example]]

### Using [[Stalled Operations]] in LSQ
"**Pre-fetching**" -> Try to load in advance but only use when at the correct point in program execution

-> While waiting, peek on all waiting ops. in LSQ
![[Pasted image 20221029054848.png|500]]
Ex : A is in cache (short latency), Y is not (long latency) 
-> Peek Y to overlap latency of load Y