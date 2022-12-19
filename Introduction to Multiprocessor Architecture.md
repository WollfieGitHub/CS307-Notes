#Course 

## Intro
Performances of single-core processors increased rapidly from the 50s to 2010 but this increased begins to flatten due to the properties of the component (overheating) based on the operating voltage we can no longer reduce and the Clock frequency we still would like to increase. This is why, to keep increasing performances with a decent cost, the demand for [[Parallel Computing]] increased.

## [[Cache Coherence II|Cache Coherence]]
Minimize the traffic on the bus by keeping track of the copies in the cache

## [[Optimizing Software]]
- False sharing
- True sharing
- Loop overheads (Loop unrolling, fusion, ...)

## [[Memory Consistency]]
1. Sequential Consistency
2. Processor Consistency
3. Weak Consistency 
4. (Language Level Consistency)
- Data Race Free Program => Sequential Consistency

## [[Synchronization]]
1. Locks
2. Point-To-Point Synchronization
3. Transaction Memory

## [[Multi-Threading]]
1. Granularity
2. Vertical/Horizontal Waste

## [[GPU]]
General execution workflow:
1. Allocate memory on the GPU
2. Copy data from CPU to GPU memory
3. Invoke the GPU kernel
4. Wait until the GPU is done (synchronization)
5. Copy the data back to the CPU's memory\

## [[CPU Scaling Trends]]