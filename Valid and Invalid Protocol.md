### Naïve Solution : Valid and Invalid Protocol
- Conceptually simple
	- Track cache block validity with a single bit in cache tag
	- Broadcast all updated values for a block
	- Other copies in the sytem are invalidated, re-read new value
- Core's actions
	- Read succeeds if valid, otherwise fetch from memory
	- Always write to he next level (don't allocate in cache for now)
- Cache's controller actions
	- When seeing write from another core, invalidate the block

