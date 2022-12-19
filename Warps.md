*Definition:* n-threads wide bundles
- 32 Threads running together : Natural choice because the pipeline is 32-wide [[SIMT]]. It's a choice made to simplify hardware scheduling :
	- Otherwise, [[SM]]s track thousands of threads. Current [[SM]]s allow 48 warps per [[SM]], to 1536 CUDA threads
	- -> Tracking memory/register readiness for 1k threads is hard

#### Implications
Since all threeads of a warp execute together, they *all* have to be ready or can't execute. If only 1 thread out of 32 is waiting (e.g., memory), [[SM]] hardware will not schedule the warp
-> Huge vertical pipeline waste

- H/W solves it with more warps to hide latency
- S/W is an even better solution
- Another problem is thread divergence (not covered?)

![[Pasted image 20221203151623.png|300]]
![[Pasted image 20221203151632.png|300]]

### Allocating Warps
- Two levels of resource allocation
	- Global allocation maps groups of warps to cores
		- "Groups of Warps" = Thread Blocks
		- Allocates registers in cores
- Instruction scheduler chooses warps to issue
	- Find warps with independent instructions