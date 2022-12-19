# Point to Point Network Organization

More scalable systems use point-to-point network
- A Messages have an explicit source and destination
- e.g., Intel Skylake uses a ring

![[Pasted image 20221011062906.png]]

- Problem for our coherence protocols
	- Each core no longer sees all of the messages
	- Need to introduce another protocol controller into the system…
## Coherence directory

- The directory tracks all caches that contain a particular block
- All messages leaving the core are first sent to the directory
- The directory takes the appropriate actions, and then responds to the original cache’s request
- The directory serves as a per-block address “ordering point”

- For now, directory is just another node in the interconnection network (other structures later)

![[Pasted image 20221011063114.png|300]]

### Example Scenario
1. P0 Read X → S
2. P1 Read X → S
3. P0 Write X
	1. Forward the write miss to directory
	2. Directory sends inv’s to other sharers → M
	3. P1 inv’s block X → I
	4. P1 acknowledges invalidate
	5. Directory responds to P0, transitions state → M
4. P1 Read X
	1. Send request to directory
	2. Directory sees P0 has the most updated copy, and forwards P1’s request → S
	3. P0 sends P1 the data → S and writes back to memory

### Entries in Directory
![[Pasted image 20221011063411.png]]

## [[Duplicate Tag Directories]]
### Cons
Associativity is bad for latencty & power
Duplicate tags require massive associativity

#### Solution : [[Sparse Directories]]
-> Split the L1 ways across multiple directory sets, allows us to reduce associativity. Sets are easier to add than ways!

![[Pasted image 20221011071319.png|500]]
-> Contention in set 0010 (3rd row) and 1110 

### [[Bit Vector Directory Entries]]

## Real-World Directory Structure
![[Pasted image 20221011072138.png|500]]
◆ L2 caches:
	◆ 512kB capacity, 64B lines
	◆ Inclusive of 64kB L1 caches
◆ L3 cache tracks which of the L2s share a block
	◆ 48 possible locations (4 chips, 12 cores each)