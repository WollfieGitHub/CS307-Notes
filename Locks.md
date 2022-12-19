## Desirable Characteristics
1. **Low Latency**
	1. Acquisition of a lock should be quick
2. **Low Traffic** 
	1. Waiting for a lock should generate little to no traffic
	2. A busy lock should be handed off between processors with as little traffic as possible 
3. **Scalability**
	1. Latency/Traffic should scale reasonably
4. **Low Storage Cost**
5. **Fairness**
	1. Avoid starvation or substantial unfairness 
	2. Ideal : Processors should acquire a lock in the order they request access

## Creating a Lock
### First Try
Loads & Check if lock is 0 : **Fail** -> Two locks can read 0 and believe the lock is free

### [[Test-And-Set]]
- Low latency (under low contention)
- High coherence traffic
- Poor scaling
- Low storage cost (one integer)
- No provision for fairness

### [[Test-And-Test-And-Set]]
- Slightly higher latency than [[Test-And-Set]] (Must test before test and set)
- Generates much less interconnect trafic
	- One invalidation, per waiting processor, per lock release
- More scalable (due to less trafic)
- Storage cost unchanged (still one integer)
- Still no provisions for fairness

### [[TS with Exponential Back-Off]]
- Same uncontented latency as [[Test-And-Set]]
- Generates less trafic than [[Test-And-Set]]
- Improves scalability (due to less trafic)
- Storage costs unchanged (still one integer)
- Exponential back-off can cause sever unfairness
	- Newer requesters back-off for shorter intervals

## Problem
- Both TS and TTS issue an instruction which is both read and write which is a challenged for pipelined processors.
- Additionally, must lock the bus (Block all ops) until the read and modify completes

## OMP Implementation
```c
omp_lock_t lock;
omp_init_lock (&lock);
#pragma omp parallel num_threads(4)
{
	int tid= omp_get_thread_num( );
	int i;
	for(i= 0; i< 3; ++i){
		omp_set_lock(&lock);
		printf("T %d : begin locked region\n", tid);
		printf("T %d : end locked region\n", tid);
		omp_unset_lock(&lock);
	}
}
omp_destroy_lock(&lock);
```