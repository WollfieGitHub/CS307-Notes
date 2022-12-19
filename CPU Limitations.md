##### Review
- Horizontal Waste : Unable to use full issue widht
	- Software not exposing enough ILP for hardware
- Vertical Waste : Whole cycle empty, nothing issued
	- Most common after long latency events

#### Limitations
- Adding ALUs has an upper limit
	- Need instructions to utilize all of the units
	- Pipeline width has diminishing benefit (recall [[Multi-Threading]])
- SMT addresses horizontal waste by bringing more independent instructions
	- Also has an upper limit : Scaling register files and [[Reorder Buffer|ROB]]

