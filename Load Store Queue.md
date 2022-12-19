Hottest point in the CPU
### Two Goals
Order memory instructions to :
1. Resolve which Load/Store addresses overlap
2. Hold all store operations until they retire

#### Address Resolution
- Address resolution is necessary to forward values

##### 1. Cross checks every entry of the LSQ against all older ones
![[Pasted image 20221020053405.png|300]]
##### 2. `Load` sets bits for *all* older `store`s they depend on
![[Pasted image 20221020053747.png|300]]
-> $Ld(A)$ sets 0 because it depends on $St(A)$ at 0
-> $Ld(B)$ sets 1 because it depends on $St(B)$ at 1
##### 3. `Store` resets its column when updating the cache
<div style="display: flex; flex-direction: row">
	<img src="C:\Users\leowo\OneDrive\EPFL_Notes\Resources/Pasted image 20221020053817.png" style="width: 50%; height: auto"/>
	<img src="C:\Users\leowo\OneDrive\EPFL_Notes\Resources/Pasted image 20221020053847.png" style="width: 50%; height: auto"/>
</div>
-> It releases its dependencies

#### Speculative Values
- We cannot write speculative values to caches
	- Speculative values = The values that ran out of order (OoO processors predict branches and speculate)
	- They wait until all prior accesses are complete
	- Otherwise they may corrupt the system's state

##### Blocking speculative stores
Reminder : 
- Instructions get an ROB entry at rename and release it when commit
- Instructions are in-Order fetches, in-order commits, Out-of-Order execute

-> Only remove an LSQ entry when store exits ROB
![[Pasted image 20221022101951.png|400]]
- When at head of ROB (blue)
	- Retires from ROB and de-allocates LSQ entry
	- Has not triggered exception or page walk