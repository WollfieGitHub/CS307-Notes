Would calling `x++` twice result in `x+2` if called by two threads ?
-> No : Could lose one update if reading local X

Trickier case : 
![[Pasted image 20221029062047.png]]
Almost any compiler can break this code !
- Variable `done` is detected as "loop-invariant" and therefore moved out of the loop and only read once.
- This `done` as part of "loop invariant code motion" and "register allocation"
- Thread 2 can loop infinitely if `tmp` reads `done=0`

-> Adding `volatile` keyword means that the variable may be changed at any time.
- Disables register allocation, code motion, and other compiler optimization

### Another Problem : Re-ordering
Gets reordered as such : 
![[Pasted image 20221029062412.png]]
- From POV of compile, `val` and `done` are independent, can re-order them if desired.
- Now what behavior is possible, especially if `expansive_func` takes a long time ?
	- Answer : `done = 1` but T2 reads `val = 0`
- This sort of reordering is both *indeterministic* and *platform dependent*
	- May not be immediatly discovered during testing
- Can fix with fences but kills code portability cross-ISA