```c
void Lock(volatile int * lock)
	int amount = 1;
	while (1) {
		if (Test_and_Set (lock) == 0) {
			return;
		}
		delay(amount);
		amount *= 2;
	}
}
```
Studies shown that using exponential waiting balances low latency and contention

1. No Contention : Acts like [[Test-And-Set]]
2. With Contention : Every test is a store miss, every unlock is a store miss, but the tests are exponentially distributed
