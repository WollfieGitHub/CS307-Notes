Historical leftover from graphics :
- Two more types of memory
	- But are not used that often
	- They are beneficial only for very specific types of apps
	- We won't talk much about them
- Constant & Texture memories
	- Global with a special cache
	- Must be set from host before running kernel
		- Read-only over the course of a kernel execution
	- Can be used to reduce pressure on global memory