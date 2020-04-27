# Chapter 3 Memory Management

## No Memory Abstraction
1. Every program access the memory by its real address.
2. The operating system still need to save the entire contents of memory to a disk file to meet the demand of running multiple programs.(P183)

## Memory Abstraction: Address Spaces
1. Problem:4
   1. The user program may trash the OS.
   2. Difficult to have multiple programs running at once.
2. Address Space: The segment that specific program can use.
   - Base and Limit Register: Dynamic Relocation(P186)
   - Give each process a separate address space.
3. 