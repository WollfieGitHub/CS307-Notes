- When ROB is completely full, no instructions can be fetched anymore.
- Why ? Because Load A waits for Store X, Store B in :
![[Pasted image 20221029054848.png|500]]

-> Let the Loads (Load A in this case) go if they are targetting different addresses than the stores we are waiting for