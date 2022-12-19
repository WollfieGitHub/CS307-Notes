# MSI Protocol

Modified, Shared, Invalid

- Modified (Dirty)
	- This cache has updated the value and holds the authoritative copy
	- Memory is stale, needs to be updated on eviction
- Shared (Valid)
	- One or more caches hold the value, read permission only
- Invalid

## Core :
- Must obey the permissions implied by a cache state
	- e.g., Cannot write in a cache block with Shared

## Cache Controller :
- Distinguish between read and ”read with intent to modify”
	- Load → read → Shared
	- Store → “read to modify” → Modified
- On a write to a block in S, send an invalidate to other caches
- When seeing a read to a block in M, put local data on the bus

## MSI State Diagram
On Modified :
- If R/W from processor, nothing happens, has the authoritative value
- If BusRdX (with intent to modify), invalidate and write data back to the bus in response for the bus to have the copy
- If BusRd (No intent to modify), then go into shared as other caches will have the value
On Shared :
- On BusRd or PRd, nothing happens, has the good value
- On PWr, invalidate all other copies caches might hold and go into modified as we now have the authoritative value
- On BusRdx, or BusInv, invalidate
On Invalid :
- On PRd, fetch the data in the bus and go into shared
- on PWr, read with intent to modify and go into modified as we now have the authoritative value
- Otherwise do nothing as we don't have the value
![[Pasted image 20221011061607.png]]
