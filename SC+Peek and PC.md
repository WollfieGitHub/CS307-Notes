## PC:
![[Pasted image 20221029060734.png|500]]
## SC + Peek:
![[Pasted image 20221029054848.png|500]]
## Difference :
![[Pasted image 20221029060529.png]]
Compare the two LSQs : 
- In SC+Peek, Load A hits L1 but still had to wait
	- PC allows it to bypass the ordered stores to X, B and A
- In PC, why is Load Y in L1 miss, not a peek ?
	- Independent addresses, so the load is actually issued
	- If it hit the L1, another instruction could read the result
## Performance comparison :
![[Pasted image 20221029060935.png]]
