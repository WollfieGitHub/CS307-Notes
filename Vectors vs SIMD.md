- [[SIMD]] instructions have configurable width and actually move data there
- Vector processors operates in *strides*
	- Gather elements from memory, operate in prallel on all of them : No need to change code for different [[SIMD]] implementations
	- GPUs are more like vector processors : Since instructions are execute all together, operations happen in large vectors

#### Example : Image Processing
Operation to do a fade out of an image : Take every pixel and multiply by a constant (e.g. 0.7)
- Single thread programming : Loop over all pixels
- Parallel language : Divide the work among a bunch of threads