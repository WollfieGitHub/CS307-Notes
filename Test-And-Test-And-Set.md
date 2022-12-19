TTS Lock : Reduce coherence traffic compared to [[Test-And-Set]] lock by testing locally first

#### Lock
```c
void Lock(int * lock) {
	while(1) {
		while(*lock != 0) { // Additional test
			// While another processor has the lock...
		}
		// When lock is released, try to acquire it
		if(Test_and_Set(lock) == 0) {
			return;	
		}
	}
}
```
#### Unlock
```c
void Unlock(volatile int* lock) {
	*lock = 0;
}
```

## Coherence Traffic
![[Pasted image 20221121105104.png]]