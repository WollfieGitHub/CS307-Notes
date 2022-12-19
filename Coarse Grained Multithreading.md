![[Pasted image 20221122143132.png|300]]
- Switch to a new thread on a long latency event
	- Pay a small cost to switch in a new context
	- Addresses purely vertical waste
- May increase utilization
	- But required to swap on long latency events

## Performance
Critical decision is when to switch threads
- When current thread's utilization is about to drop (e.g., L2 miss)
- Requirements for improving througput :
	- $\text{Thread Switch} + \text{Pipe fill time} << \text{Blocking Latency}$ i.e. need useful work to be done before other thread comes back
	- Fast thread-switch : Multiple register branks
	- Fast pipe fill : Short pipe
- *Pro* : Small changes to existing hardware
- *Con* : Single-thread performance suffers

## Implementation
![[Pasted image 20221122143250.png|500]]

## Examples
- Macro-dataflow machine
- MIT Alewife/SPARCle
- IBM Northstar

## Exercise
![[Pasted image 20221122143623.png]]