Peek at L1, see if address from LSQ is already there.
- If not (miss), fetch the block from lower level into L1
- If so, do **not** load the value into the core

Fetching blocks into L1 from lower levels (or other L1's) does not impact order or atomicity because no values more between the core & L1

Helps overlap latency : We can fetch as many bocks in parallel into L1 as needed

### Does it violates SC ?
Example with : 
```java
Thread1 :         Thread2
// A = r_0 = 0    // B = r_1 = 0
(S_0) A = 1;      (S_1) B = 1;
(L_0) r_0 = B;    (L_1) r_1 = A;
                  
print(r_0);       print(r_1);
```
-> **We can't observe $r_1 = r_2 = 0$** : If Load(B) peeks, value brought into cache is B = 1 or B = 0 (depending on
T2). Still needs to read B when it executes (S1 may have invalidated it)