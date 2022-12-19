## Aligned, Consecutive
- Warp requests 32 **aligned**, **consecutive** 4-byte words
- Addresses fall within one cache line
	- Warp needs 128 bytes
	- 128 bytes move accross the bus on a miss
	- Bus transfer utilization: 100%
![[Pasted image 20221211043903.png|400]]

## Aligned, Permuted
- Warp requests 32 **aligned**, **permuted** 4-byte words
	- Warp needs 128 bytes
	- 128 bytes move accross the bus on a miss
	- Bus Utilization: 100%
![[Pasted image 20221211043931.png|400]]

## Misaligned, Consecutive
- Warp requests 32 **misaligned**, **consecutive** 4-byte word
	- Warp needs 128 bytes
	- 256 bytes move accross the bus on a miss
	- Bus Utilization: 50%
![[Pasted image 20221211044121.png|400]]

## Same 4 bytes
- **All** threads in a warp request the **same** 4-byte word
	- Warp needs 4 bytes
	- 128 Bytes move accross the bus on a miss
	- Bus Utilization: 3.125%
![[Pasted image 20221211044319.png|400]]

## Scattered 4 bytes
- Warp requests 32 scattered 4-byte words
- Addresses fall within N cache lines
	- Warp needs 128 bytes
	- Nx128 bytes move accross the bus
	- Bus Utilization = $\frac{128}{N\cdot128}$ , for N=32 -> 3.125%
![[Pasted image 20221211044521.png|400]]