- Need to express parallelism in [[SIMT]]
- Break down programs into small **Kernels**

- Extension of C to allow GPU programming without explicit graphics mode abstraction (OpenGL)
- Low abstraction distance to actual device : Programmers can tune performance for hardware

## Example
- Need to identify the code as GPU-side : use the `__global__` keyword
- Each thread does the following :
	- Gets its element id
	- Check it's in array bounds
	- Compute output and returns

```c
__global__
void image_fade(int* input, int* output, int a_size, int fade) 
{
	int element_id = ...; // Assume this works, more to come...
	if (element_id >= a_size) { return; }
	
	int value = input[element_id] * fade;
	if ( value > 255 ) { value = 255; }
	output[element_id] = value;
} 
```
- [[GPU]] is like a powerful coprocessor
	- Has its own ISA and memory
	- The [[CPU]] explicitly control it

### General Execution Flow:
1. Allocate memory on the GPU
2. Copy data from CPU to GPU memory
3. Invoke the GPU kernel
4. Wait until the CPU is done (synchronization)
5. Copy the data back to the CPU's memory
![[Pasted image 20221203152546.png|300]]

### 1. GPU Memory Allocation
- GPU don't run an operating system
	- No unified memory manager, up to the programmer
	- Corollary: You code can step on other people's code
- CPU allocates, copies and deallocates.
	- `cudaMalloc(..)` and `cudaFree(..)`
![[Pasted image 20221203152752.png|400]]

### 2. Memory Copy from CPU to GPU
- When copying data to the device, the `cudaMemcpy()` is **ASYNCHRONOUS**, CPU keeps running
	- It is Direct Memory Access != MMIO : There is a specialized unit that does the copy
#### GPU Pointer
It is **NOT** a regular C pointer ! If you dereference it, the behavior is undefined. 
- The host pointer is on the CPU's stack but it serves as a simple name for the CUDA library. When calling `cudaMalloc(..)`, device driver tells the GPU to "please allocate `SIZE_N` bytes of memory"
- Don't forget : You must copy to/from the GPU !
```c
...
cudaMemcpy((void*) gpu_array, /* Dest */
		  (void*) host_array, /* Src  */
		  SIZE_N,             /* NBytes */
		  cudaMemcpyHostToDevice); /* Direction */
```
- The copy *direction* matters for your results, probably use the one of the first 2 kinds :
```c
enum MemCpyKind {
	cudaMemcpyHostToDevice,
	cudaMemcpyDeviceToHost,
	cudaMemcpyDeviceToDevice
};
```

Now we have everything :
```c
__global__ void image_fade( .. ) { .. }

int main( .. )
{
	cudaMalloc(..);
	cudaMemcpy(..);
	dim3 thrsPerBlock(3,4); // 3x4
	dim3 nBlks( 2,3 ); // 2x3
	image_fade<<< nBlks, thrPerBlock>>>(args); // This is also asynchronous
	/* the <<<>>> is important here ! */
}
```
- Organize each threads into "blocks" (TBs)
	- Each TB contains a bunch of threads
	- The TBs are organized as a **grid**
	- Fun indexing - the block can be 1D, 2D or 3D !
- When you call a kernel, you specify :
	- The number of TBs and threads per TB

### 3. Thread Organization
For the example above, with 
```c
dim3 thrsPerBlock(3,4); // 3x4
dim3 nBlks( 2,3 ); // 2x3
```
It specifies the layout below: 
![[Pasted image 20221210160958.png|200]]
- Grid = 2x3 Blocks
- Blocks = 3x4 Threads

#### Occupancy
Each thread and block knows where it sits in the layout
For max performance, GPU should be fully occupied :
- Requires to think about your kernel’s operations
	- Which warps are long latency?
	- How many warps can be swapped in while this warp is waiting?
See -> [[GPU Block Sizing]]
Can use : "CUDA Occupancy Calculator" and download spreadsheet

### 4. Transferring Data Back to CPU
- Copying back to the CPU is **SYNCHRONOUS**
- Not the same as copying to the GPU
```c
int main(...)
{
	...
	cudaMemcpy((void*)host_array, /* DEST */
	(void*)gpu_array, /* SRC */
	SIZE_N, /* NBYTES */
	cudaMemcpyDeviceToHost /* DIR. */);
}
```

### 5. Host Synchronization
To invoke multiple kernels, you may need to synchronize between
(depending on what they
-> Waits until all previous copies and kernels complete with `cudaDeviceSynchronize()`
```c
int main(...)
{
	cudaMalloc(...);
	cudaMemcpy(...);
	image_fade <<< nBlks, thrsPerBlock >>> ( );
	cudaDeviceSynchronize(); // HERE
	other_kernel <<< nBlks, thrsPerBlock >>> ( );
}
```
- Thankfully, CUDA provides a built in barrier: `__syncthreads()` which is much faster than `cudaDeviceSynchronize()` which requires 2 roundtrips to GPU
- All threads in the **same thread block** wait here

#### Consistency
CUDA implements **WEAK Consistency**

### 6. Indexing in Kernel
![[Pasted image 20221210161901.png|400]]
Same goes for `y`

```c
x_glob = (blockIdx.x * blockDim.x) + threadIdx.x ;
y_glob = (blockIdx.y * blockDim.y) + threadIdx.y ;
array_index = (y_glob * ROW_SIZE) + x_glob;
```

