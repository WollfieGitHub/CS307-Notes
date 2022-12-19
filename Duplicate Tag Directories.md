## Duplicate Tag Directories
### Duplicate Tag Directory
Store all tags that are present in the L1 Caches
-> The set-mapping function must be identical
-> 2 Processors with 4way cache means that each directory must be 8way associative

### Distributed Duplicate Tag Directories
Centralized directory can also be a source of contention, just like a bus.
- Insight: L2 caches are multi-banked and distributed across chip
- Solution: Distribute the directory with the L2 banks
- Each block has a home node determined by address interleaving
- Each directory still requires associativity equal to sum of L1 caches
#### Accessing Caches
![[Pasted image 20221011071140.png|500]]

![[Pasted image 20221011071927.png|500]]
◆ Divide the L1 sets across L2 banks
◆ Use L1 low-order index bits to find the directory
◆ Rest of the L1 index bits + tag bits for directory
### Cons
Associativity is bad for latencty & power
Duplicate tags require massive associativity

#### Solution
-> Split the L1 ways across multiple directory sets, allows us to reduce associativity. Sets are easier to add than ways!

![[Pasted image 20221011071319.png|500]]
-> Contention in set 0010 (3rd row) and 1110 