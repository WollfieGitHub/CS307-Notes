# Parallel Computing

- Instruction level parallelism
- Task level parallelism
- Data level parallelism

### Connectivity
Up to multiple chips/board
- Shared memory

Across board :
- Distributed memory (R/W only local)
- Pass messages through a network

#### Moderns servers are NUMA
(Non Uniform Memory Access)
- First level cache ~ 2cycles
- Second level cache ~ 10cycles
- Third level cache ~ 50cycles
- Off-chip own DRAM ~ 150cycles
- Off-chip others' DRAM ~ 300 cycles

Content everywhere
- On-chip network
- Off-chip network
- Cache ports, memory channels and cache/memory controllers

## Example : Deep Learning
![[Pasted image 20220922050416.png|150]]
![[Pasted image 20220922050506.png|150]]

## Why Parallelism ?
1. The free energy lunch is over
	1. End of frequency scaling
	2. Instruction Level Parallelism Tapped out
2. Moore's law Reinterpreted
	1. Chip density increase slowly
	2. Clock speed does not

-> Parallelism is a key solution to achieving higher performance

### How
Idea #1 : Use more transistors but on simpler cores
Idea #2 : Add ALUs to increase compute capability
- SIMD Processing : Single Instruction Multiple Data
- -> Broadcast same instruction to all ALUs
	- -> Vector Program (using AVX Instructions)

[[GPU]] = How to scale SIMD to *thousands* of [[ALUs]]

#### Programming
- Use existing parallel code through libraries : Spiural, ScaLAPACK, BLISS
- Writing in a parallel programming Langauge : Java, Scala
- Writing in compiler directives : [[Open MP]], Intel Threading Building Blocks
- Writing using a threading library : MPI, PVM, PThreads
- Domain Specific Languages (DSLs) : Spiral, Halide, CUDA

## Principles of Par. Comp.
1. Balance Workload : Give each parallel task the same rough amount of work
2. Reduce Communication : 
	1. Balance computation time and communication time
		1. Computation -> Useful work
		2. Communication -> Overhead
3. Reduce extra work
	1. Scheduling Tasks on processors, OS, etc...
4. These are at odds with each other


### [[Amdhal's Law]]
- Important to improve that which dominates the execution time

## Shared Memory vs Messaging
### Shared memory
- Threads communicate
	- reading/writing to shared vars
	- Manipulating synchronization primitives
- Shared var are like a big bulletin board
- Any thread can read or write

### Messaging

## [[Open MP]]