- Flags mark updates to data.
- Barriers introduce planned "stalls" in threads
	- Wait for all threads to read their input before start
- Technically equivalent, barriers use flags
	- Flags also have no built-in support for thread blocking

## Building a centralized Barrier
- How many threads to wait for
- Flag to tell threads to proceed or wait
- And a lock to protect it all

```c
struct Barrier_t {
	Lock aLock;
	int counter;
	int flag;
};
```

### Algorithm
#### First try
1. Clear the flag, all incoming threads should wait
2. Every arriving thread increments `counter`
3. Last thread see `counter == num_threads`
4. Set the flag, all threads leave
-> Does not work, a thread can set the flag and reach the next barrier alone which will clear the flag

#### Private Sense Reversal
- Keep a private copy of flag, the first thread to arrive writes its private copy
`int local_sense = 0; //private copy`

##### Implementation
```c
void senseBarrier(Barrier_t* b, int num_threads) {
	local_sense = !(local_sense); // (Step 1)
	lock(b->aLock);
	int num_waiting = ++(b->counter); // (Step 2)
	
	if( num_waiting == num_threads ) { // (Step 3)
		unlock(b->aLock);
		b->counter = 0;
		b->flag = local_sense; // (Step 4)
	} else {
		unlock(b->aLock);
		while( b ->flag != local_sense ); // spin
	}
}
```
##### Analysis
- O(P) traffic on a bus
	- 2P write transactions to obtain barrier lock and update counter
	- 2 write transactions to write flag + reset counter
	- P-1 transactions to read updated flag
- Still we serialize on a single shared variable
	- Latency $O(P)$

#### Combining Trees
- Makes better use of parallelism in interconnect topologies
	- Latency **$Log(P)$**
	- Strategy makes less sense on a bus
![[Pasted image 20221122045008.png|350]]
- When processor arrives at barrier, `atomic_incr` parent counter
	- Process recurses to root
- Release : Begin from root, notify children of release