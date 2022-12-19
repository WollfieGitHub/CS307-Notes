New drawing at the bottom of [[Superscalar Processors]]

- Pipeline allows multiple instructions to be in-flight
	- Perfect cases reduces instructions per cycle to 1
	- Assuming five stages (Fetch, Decode, Execute, Memory, Write)

- Assuming a perfect pipeline : Scalar IPC = 1
- Duplicating the pipeline halves it : IPC = 2

## Pipeline Bubbles decrease IPC
1. Structural hazards : Not enough resources
2. Branch control flow : Branch prediction may be wrong
3. Data dependences : Cache prefetching only useful for trivial patterns.

## Out-of-Order Execution
1. Fetch in-order
2. Execute out-of-order
3. Reconstruct order on retirement

-> Expose parallelism for long-latency events
### Hardware Waste
#### Vertical
Whole cycle empty, nothing issued
- Most common after long latency events

#### Horizontal
Unable to use full issue width
- Software not exposing enough ILP for hardware

### Independence
-> We need to find independ instructions in threads
- If no instructions, try another thread
	- Requires having thread context-awareness in hardware
	- Context entails : PC, SP, register file, interrupt descriptors
- Give the issue slot (each cycle) to another thread
	- Assuming multiple context, this is easy
	- Targets *Vertical Waste*
- Mix instructions from multiple threads per issue slot
	- Problems : Scheduling policy (Which thread to choose) ? fairness ?

### Trade-Off
- Fundamental
	- Multithreading **always** increases single-threaded latency
	- But increases pipeline utilization, higher throughput
- Likely an acceptable sacrifice
	- If the program can't use hardware anyway...

### Storing Multiple Contexts
- Each threads needs **architectural** state (Regs, PC, Stack, Interrupt Tables, ...) which's cost is ~KB of SRAM
- Threads share the memory hierarchy
	- Potential contention in data cache
	- Contention in instruction cache

## Granularity
- [[Coarse Grained Multithreading]]
- [[Fine Grained Multithreading]]
- [[Simultaneous Multithreading]]

### FGMT vs CGMT
#### Thread Switch Policy
- Most historical designs of [[Fine Grained Multithreading]] is round robin
- [[Coarse Grained Multithreading]] swaps on long latency event
#### Latency
- [[Fine Grained Multithreading]] addresses much shorter latencies : Hardware costs to keep all context immediately ready
#### Parallelism
- For threads with abundant parallelism, [[Fine Grained Multithreading]] could suffer
	- Can introduce "flexible interleaving" to solve
	- One thread could remain scheduled for many cycles