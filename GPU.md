Graphic Processing Unit

##### Programming GPU : Guidelines
- Check correctness often
- Make it work before you make it fast

[[CPU Limitations]] : Has an upper limit
-> **One Instruction, All ALUS**

## GPUs
- Control all of the ALUs with one instruction
	- This is a vector processor (built first in 1972, 1976)
- We've gone from "program threads" to "SIMD threads" running in lockstep

-> [[Vectors vs SIMD]]

### Massively Parallel Hardware : GPUs
- Scale pipeline with e.g., 1k ALUs
- Key insight : use a single instruction stream because we know all threads are doing the same thing
-> Problem : Memory are long latency ops.
- Solution : Use huge degree of [[Multi-Threading]] : Every clock cycle should have a thread ready to execute

1. Make **simple cores** : In-order pipelines
2. **Thousands of ALUs** on the die : 
	- NVIDIA Volta : 5.5k integer and 2.5k FP Alus
	- NVIDIA call these cores but they are not similar to CPU cores
3. **Share cores** among thousands of threads
4. Take Throughput-latency trade-off to extreme
	- Trillions of integer operations per second
	- But huge single thread latency
![[Pasted image 20221203144703.png|300]]

#### Streaming Multiprocessor (SM)
-> [[SM]] : Streaming Multiprocessor

#### Parallelism Model 
-> [[SIMT]] : Single Instruction, Multiple Threads

## Thread
- On a [[CPU]], a thread has its **own execution context** : Stream, stack, program counter...
- On a [[GPU]], all threads **share an execution context** : Stack, Instruction Stream...

## [[Warps]] 
- Group of 32 threads that must execute together

# [[CUDA]]
Introduction to CUDA Programming

## [[GPU Optimizations]]