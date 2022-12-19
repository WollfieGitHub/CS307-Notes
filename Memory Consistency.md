```java
Thread1 :         Thread2
// A = r_0 = 0    // B = r_1 = 0
(S_0) A = 1;      (S_1) B = 1;
(L_0) r_0 = B;    (L_1) r_1 = A;
                  
print(r_0);       print(r_1);
```

- It is possible to read `(r_0 = 0, r_1 = 0)`
- Because Non-Blocking caches proceed while msgs. are in network, when `(Write A, Write B)` is issued and processed, `(Read A, Write B)` is processed and reads `(0, 0)`

- We must design a robust model which tells each model waht can and can't happen

## [[Sequential Consistency]] (SC)
With this model, the possible outputs to the previous program are `(0, 1)`, `(1, 0)` and `(1, 1)`

## [[Processor Consistency]] (PC)
Difference between [[SC+Peek and PC]]

## [[Weak Consistency]]
Relax even more : only obey uniprocessor constraints for correctness
- Used in ARM

## [[Language Level Consistency]]