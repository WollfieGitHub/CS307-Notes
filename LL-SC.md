Load Locked & Store Conditional

Load-Locked : `ll r_x, mem[addr]`
Store-Conditional : `sc r_x, mem[addr]`

Inteacts with coherence protocol to guarantee no intervening write to [addr]

LL puts the address and flag into a link register
- Invalidation or evictions of that address clear the flag and the SC will then fail
- Signals that another core modified the address

## Does concurrency breaks mutual exclusion?
```x86asm
Lock:
	ll r2, [X]
	cmp r2, #0
	bnz Lock // if 1, spin
	addi r2, #1
	sc [X], r2
```
- Processors 0 and 1 both execute the code, with cache block X beginning in `Shared`
- Both `ll [X]` read 0
- Both begin to execute `sc [X]`
- Does it break mutual exclusion ?
-> No : Cache coherence ensures the propagation of values to a single address. When both processors try to `BusInv`, one of them will "win" and clear the other's flag register and its store will fail as well.

## Example use
```x86asm
Test_and_Set
	ldl_l t0, 0(t1) ; load locked, t0 = lock
	bn t0, L1 ; if not free, loop
	lda t0, 1(0) 0); t0 = 1
	stl_c t0, 0(t1) ; conditional store, 
		; lock = 1
	beq t0, Test_and_Set ; if failed, loop
Release:
	stl 0, 0(t1)
```
