# Sparse Directories

-> Split the L1 ways across multiple directory sets, allows us to reduce associativity. Sets are easier to add than ways!

![[Pasted image 20221011071319.png|500]]
-> Contention in set 0010 (3rd row) and 1110 

>By reducing associativity, we introduce conflict

-> Eviction policy ?
## Handling Directory Conflicts
We need a mechanism to handle directory conflicts while maintaining coherence
1. Recall the block from all upper-level caches
2. Fall back to broadcast (if conflicts are rare)

Over provision number of sets :
-> Use more tag bits to index
![[Pasted image 20221011071717.png|500]]

- Pro : Low conflict rate
- Con : Large area overhead
