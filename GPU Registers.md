- Stack variables declared in kernels
- Fastest access to data
	- 10x faster than [[GPU Shared Memory]]
- Bandwidth: A few 10TB/s
- Fundamental challenge in GPU microarchitecture

### Example
Fermi architecture :
- 32K 32-bit registers per SM
- 48 warps per SM
	- 1536 threads per SM -> 21 registers/thread
- 2 MB register file
	- 16 SMs -> 128 KB per SM