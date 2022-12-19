![[Pasted image 20221122053633.png|300]]

```java
void editHash(HashTbl tbl, int key) {
	// read objects
	HashObj obj = tbl.get(key);
	synchronized(obj) {
		// update
		obj.update();
	}
}
```

- Contention for objects only
- Lots of concurrency exposed
	- Limited only to updating and reading each object
	- Cannot actually modify the hash table structure

## Idea of Contention
### Uniform Accesses
- Uniform distribution of accesses
- One million buckets in the hash table
- Probability of conflict = $10^{-6}$
### Popular objects
- One million buckets, 0.01% are celebrities
- 90 % accesses go to celebrities
- Probability of conflict = $8.1 \cdot 10^{-3}$
