	# Cache Coherence II
Revisit of [[Cache Coherence]] but for [[Introduction to Multiprocessor Architecture]] course

## Assumptions
All memory accesses are atomic & program order
All core see all access immediatly (for now)

-> Look at coherence in L1 Data caches

Incoherence is intrinsically due to sharing data
- More specifically : updating a value and reading a different copy

### Expectation
> Any core P, reading address X, will get the last value written to X

### Goal
The cache coherence should be invisible. 
Ensure a **unified** view of each memory location in **isolation**

## Tracking valid copies
### Single Writer, Multiple Reader (SWMR)
- All protocols we study in this course implement this policy
- Either the last writer or any reader has a valid copy

## How
1. Assume a bus connects cores and memory
2. Add independent "cache controllers" to each core
	- Each controller is a FSM
	- The controller sees all remote processor actions
	- Take coherences actions without stopping processor
	- Actions depend on block's state and protocol

### Solutions
1. Naive : [[Valid and Invalid Protocol]]
	- Would use too much of the BUS bandwidth
2. Adding a [[Private Written Copy]]

## Remaining Problems
- The bus still can only support 3 cores assuming 40% utilization

### [[P2P Network Organization]]