How to synchronize in hardware

## Methods
### Shared Memory Synchronization
Threads read and write to global variables in shared memory

1. Mutual Exclusion
	1. [[Locks]]
2. Point-to-point synchronization
	1. [[Flag-Based Synchronization]]
	2. [[Barriers]]
3. Software methods (Not in lectures)
	1. Queue, Counters, Software Pipelines
4. [[Transactional Memory]]

### Message Passing
Threads can communicate via explicit messages (Synchronization becomes inherent to sending and receiving)

## Objectives
1. *Low Overhead* : Synchronization with high overhead can limit scalability 
2. *Correctness* : Synchronizations failure are difficult to debug 
3. *Coordination of Hardware and Software* 
	- Software semantics must be specified to prove correctness
	- Hardware can often improve efficiency

## Phases
1. Acquire Method : How thread attempt to gain access to the resource
2. Release Method : How to enable threads to access the resource
3. Waiting Algorithm : How threads wait for access to the resource

## OMP Implementation
Critical Section
```c
#pragma omp critical 
{
	/* Critical Code Here */
}
```

Atomic Execution
```c
#pragma omp atomic
/* Update Statement Here */
```

Barriers
```c
#pragma omp parallel {
	int result = heavy_computation_part1();
	
	#pragma omp atomic
	sum += result;
	
	#pragma omp barrier
	heavy_computation_part2(sum);
}
```

Single Threaded Core
```c
#pragma omp single
{
	/* Only executed once */
}
#pragma omp master
{
	/* Only executed by master*/
}
```

