![[Pasted image 20221122143657.png|300]]
- Cycle between multiple threads periodically
- Idea : Eliminate "switch time" by keep all thread's state hot

## Performance
- No critical decision to take
- Requirement for improving througput
	- Enough threads to eliminate vertical waste
	- e.g., Pipeline depth of 8, using 8 threads is not enough
- Compensate for hardware cost in other areas
	- e.g., Bypass network may not be needed

![[Pasted image 20221122143902.png]]

### Advantages of FGMT
- With flexible interleave
	- Reasonable single thread performance
	- High processor utilization (Especially in case of many threads)
- Without flexible interleave
	-  Suits "regular" applications with repetitive latencies
### Drawbacks of FGMT
- More complicated hardware to track dependences
- Many more contexts means we need many registers
- Without flex. interleaving, limited single thread perf.
- Still **cannot** address horizontal waste

## Implementation
![[Pasted image 20221122143726.png|500]]