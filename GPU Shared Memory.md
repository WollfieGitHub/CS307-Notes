Per Block :
- Shared within a thread block
- Managed by the programmer
- Very fast, located in the SM
	- Latency: ~5ns
	- Bandwidth: ~1 TB/s
- Same HW as L1 cache (64KB)
- Inter-thread communication
![[Pasted image 20221211040750.png]]

## Memory Conflicts
Organized in 32 banks
- Each bank can service one address at a time
- At most 32 simultaneous accesses
Multiple simultaneous accesses to the same bank
- Different 4 byte word: 
	- Bank conlict : Conflicting accesses are serialized
- The same 4 byte word:
	- Multicast: 1 fetch (Could be different bytes within the word)
![[Pasted image 20221211041740.png|400]]
![[Pasted image 20221211041758.png|400]]

### Bank Mapping
- Each bank has BW of 32 bits per clock cycle
- Successive 32-bite words are assigned to successive banks
	- Memory interleaving
- Modern GPU have 32 banks
- So `bank = address % 32`

### Solution
1. Pad the rows: Add n elements to the end of each row
2. Transpose the array first: Bank conflicts on transfer but might save possible later conflicts
3. Operate on columns instead of rows: No conclict
![[Pasted image 20221211043521.png|150]]

