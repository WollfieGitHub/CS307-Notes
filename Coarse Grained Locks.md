```java
void editHash(HashTbl tbl, int key) {
	synchronized(tbl) {
		// read objects
		HashObj obj = tbl.get(key);
		// update
		obj.update();
	}
}
```

- Simple Implementation
- Lot of Contention
- No concurrency *inside* the datastructure

