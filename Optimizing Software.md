# Optimizing Software

Important to keep in mind [[Cache]] misses because they are slow.

## Misses due to coherence :
Coherence add extra misses due to transfers
- True sharing → Producer/consumer communication when processors read and write the same variable
- False sharing → Processors updating different data which are placed in the same cache block

### Example - True Sharing : Histogram
→ Count iterences of characters in a text and put them into buckets corresponding to each characters

#### 1. Divide up Input Data in Advance
Here we divide the data structure
Threads update their own buckets and merge after
→ Requires another loop 

#### 2. Privatize results when possible, reduce communications between threads
Be careful about `critical` sections
They lock and unlock the piece of code that they mark, making threads wait
→ Divide the buckets by the threads which eleminate need for any critical section

### Example - False Sharing : Matrix Multiplication
Data residing in same block overriding all the block when we needed what's in it

#### Solution 1: Use Data Padding
That is, for a given data, place garbage after it to fill a cache set
**Con** : This fills the cache with garbage and implies more capacity/conflict misses from the cache

#### Solution 2: Use Tiling
Compute data with close temporal & spacial locality :
In a matrix, compute with submatrix instead of with blocks and columns 

## Overhead
- Fork and joins come with an overhead
- If a parallelized loop has too fews iterations, use the `#pragma ... if(var > ...)` argument
##### Inverting loop helps performance :
```c
for (i =1; i <n; i)
	#pragma omp parallel for
	for(j=0; j<m; j++)
		a[i][j]=2*a [i - 1][ jj];
```
Becomes
```c
#pragma omp parallel for
for(j=0; j<m; j++)
	for (i =1; i <n; i)
		a[i][j]=2*a [i - 1][ jj];
```

##### Maximize the parallel region :
```c
#pragma omp parallel for
for (. . .)
#pragma omp parallel for
for (. . .)
#pragma omp parallel for
for (. . .)
```
Becomes 
```c
#pragma omp parallel
{
	#pragma omp for
	for (. . .)
	#pragma omp for
	for (. . .)
	#pragma omp for
	for (. . .)
}
```

![[Pasted image 20221013111833.png|500]]
Basic sequential for loops come with an overhead because the cache tries to predict and might bring wrong data.

##### Loop Fusion
Combine two loops into one 🤯

##### Loop Fission
Split one loop into two so that it improves L1 data and instruction cache miss rate

##### Parametrized Sheduling
- Static 
	- Low overhead
	- May exhibit high workload imbalance
- Dynamic 
	- High overhead
	- Can reduce workload imbalance
- Guided
	- Less overhead than Dynamic
	- Comparable to dynamic in reducing imbalance
### More General Work Scheduling 
![[Pasted image 20221013112639.png|]]