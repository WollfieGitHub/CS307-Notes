Basically, how can compilers and programming language make [[Weak Consistency]] practical to use

## [[Language Level Consistency Motivation]]
- `volatile` keyword : Signals that the value can change at any time and disables register allocation, code motion...
- Memory fences : 
	- *Compiler-level* : Does not generate any additional instructions. This is merely a hint to not reorder memory operations : `asm volatile("":::"memory");`
	- *ISA-Level* : Insert an explicit memory fence instruction to prevent reordering of loads relative to stores : `__sync_synchronize()` or `asm volatile("mfence":::"memory")`

### Problem
- There are 2 or 3 layers of potential re-ordering in between the programmer and the memory.
- Fixing it requires the programmer to specifically direct the hardware, know all low-level details.

-> Provide an end-to-end guarantee
- Given a program written "correctly", the compiler, ISA, and Hardware will guarantee certain memory ordering
- Challenge : Agree upon what a correct program is (Took 10+ years of papers from experts)

## Guarantee
The most intuituive memory model still remains [[Sequential Consistency]]
- Given many threads, pick one of them, execute N memory operations from it and move to the next one
- Didn’t we previously deem SC as too slow?
	◆ We talked about implementing it for every memory op.
	◆ Key difference now is that we only care about certain memory operations

## Grand Compromise with Languages
Programmers must write "correctly synchronized" or "well-labelled" code, containg **no data races**

>[!IMPORTANT]
>This is called "[[Data Race]] Free => [[Sequential Consistency|SC]]" meaning that the implementation of [[Memory Consistency]] does not matter as long as the program is DRF 

- Removes burden from programmer to directly insert fences and barriers into their code
- Even on hardware that uses relaxed or weak ordering, our program sees it as [[Sequential Consistency|Sequentially Consistent]]
- Calls to functions, or blocks of code appear atomic to external thread (**|!|** Only under DRF condition still)

### Tools for DRF
1. Synchronize blocks
2. (newer way) Atomic statements/variables
