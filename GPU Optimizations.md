1. Optimize algorithms for GPU ([[SIMT]])
2. Take advantage of on-chip shared memory
3. Optimize memory access pattern for coalescing
4. Use paralellism and resources efficiently

## Algorithm
- Maximize parallelism : 
	- Avoid thread sync if possible
	- Minimize control-flow divergence
- Maximize arithmetic intensity (compute/access)
	- GPU spends its transistors on ALUs, not memory
		- -> Recompute rather than cache
	- Avoid CPU-GPU data transfer:
		- Do more computation on GPU
		- Or compute on CPU if low parallelism

### Control-Flow Divergence
![[Pasted image 20221211040020.png|100]]
- Happens when threads in the *same* warp execute different paths
	- Execute one path at a time :
		- Diverging threads will be disabled
		- Limits parallelism and lowers performance
	- Different accross warps is OK : 
		- `if (threadIdx.x / WARP_SIZE > 2) { . . . } ` is in different warp

## GPU Memory System
1. Global and Local memories
2. Shared memory and L1 cache
3. Registers
4. Constant & Texture memories
5. L2 Cache

Specialized for graphics data : Global, Constant, Texture...
Caches : Not always coherent, not latency

- [[GPU Global Memory]] : Per Application
- [[GPU Shared Memory]] : Per Block
- [[GPU Local Memory]] : Per Thread
- [[GPU Caches]]
- [[GPU Registers]]
- [[GPU Constant & Texture]]

### Optimization : Exploit Shared Memory
Hundreds of times faster than global memory
Use shared memory as a managed scratchpad:
- Bring data in from global memory
- Operate in there (reuse)
- Write results back to global memory
Shared memory is fast as long as there are no bank conflicts
(See [[GPU Shared Memory]])

### Optimization : Coalesced Memory Accesses
Memory accesses
- A single coalesced access for contiguous, aligned in 128 bytes region
- Otherwise split into multiple 128B accesses
Coalescing greatly improves throughput
- Order of magnitude when using global memory
- Helps avoid conflicts when using shared memory
Critical to (global) memory-bound kernels
- Structure of arrays better than arrays of structures
- If not, read/write through shared memory

#### [[Coalesced Memory Access Example]]

### Optimization : Use Parallelism Efficiently
Partition computation efficiently
- To keep the GPU multiprocessors equally busy
- Many threads, many thread blocks
Keep resource usage low
- To support multiple active thread blocks per SM : Reg, shared memory...
For max perf., GPU should be fully occupied
- Every cycle should have a warp to issue

#### Checking Occupancy
Different GPU have different limitations
- Depends on "Compute Capability", property of GPU
Use the "CUDA occupancy Calculator" spreadsheet

#### Operations
##### Reduction
- Reduce multiple values to a single value : 
	- ADD, MUL, AND, OR...
	- Sequential : $O(n)$ steps with partial results
	- Parallel, degree 2 : $O(Log_2(n))$ steps (tree)
	- Parallel, degree x : $O(Log_x(n))$ in CUDA
###### Reduction in CUDA
- Reduction in a single Thead Block
	- Tree-like reduction, One thread -> Many Elements, Low GPU utilization because only one SM used
- Reduction in multiple Thread Blocks
	- One Block -> Portion of the input, Utilizes SM resources, Useful for large array sizes