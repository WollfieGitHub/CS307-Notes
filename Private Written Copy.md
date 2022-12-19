# Private Written Copy
- In [[Valid and Invalid Protocol]], memory has always the authoritative copy of the data

- Can a cache hold a copy, rather than the memory ?
	- Save bus traffic of always sending updates
- Needs changes to cache, and coherence protocol
	-> Must track which cache hold the copy

◆ Writeback caches have three states
	◆ Invalid, Valid, and Dirty
	◆ Valid and Invalid as before
	◆ Dirty indicates data must be *written upon eviction*

![[Pasted image 20221011060916.png|500]]

## State Diagram
##### Terminology 
- {Event -> Response}
##### Core Actions
- PRd - Processor Attemps Read Line
- PWr - Processor Attemps Write Line
##### Remote Actions
- BusRd - Obtain copy of a line with no intent to modify
- BusRd**X** - Obtain copy of a line **with** intent to modify
- BusInv - Get rid of other copies
- DataWB - Put updated value of a line on the bus

### [[MSI Protocol]]
- Pros : Save a lot of memory in the bus
- Cons : We don't know if the data is exclusive to the cache and so we miss a **BusInv** message if the data is exclusive to the cache when in **Shared** mode transitionning to **Modified** 

### [[MESI Protocol]]
