![[Pasted image 20221122144716.png|300]]
- Instructions from multiple threads in **same cycle** 

- Do we need to solve all potential dependences ? No ! Registers are physically mapped, the instructions can be issued freely
	- When both $T_1$ and $T_2$ execute `add r1, r2, r3` :
	- T1: `add r1, r2, r3` becomes: `add p4, p2, p3`
	- T2: `add r1, r2, r3`          `add p9, p8, p5`
- After allocation, **scheduler is thread agnostic**
## Performance
- Critical decision : Fetch interleaving policy
- Requirement for througput : Enough threads to utilize resources, fewer than needed to stretch dependences

### Advantages of SMT
- First design that can reduce horizontal waste

## Implementation
- Stages that duplicate their state :
	- Fetch, Decode, Allocate (One map table per thread)
	- Retire stage needs to know which register to free

### Policies to share the pipeline structures
- Static partitioning : Each threads gets 1/n the structures
- Dynamic sharing : Every entry is tagged

![[Pasted image 20221122145233.png]]