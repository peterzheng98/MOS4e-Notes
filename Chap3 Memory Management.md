# Chapter 3 Memory Management

## No Memory Abstraction
1. Every program access the memory by its real address.
2. The operating system still need to save the entire contents of memory to a disk file to meet the demand of running multiple programs.(P183)

## Memory Abstraction: Address Spaces
1. Problem:
   1. The user program may trash the OS.
   2. Difficult to have multiple programs running at once.
2. Address Space: The segment that specific program can use.
   - Base and Limit Register: Dynamic Relocation(P186)
   - Give each process a separate address space.
   1. Memory management with Bitmap: (P191)
      - First set the basic allocation block size.
      - Use a bit to express whether the block is free.
      - Slow: Allocate k-unit memory require find a run of k consecutive 0 bits in the map.
   2. Memory management with LinkedList: (P193)
      - First fit: Find the first hole that is suitable for the program.
      - Next fit: Same as first fit but record the place where the free hole is found.
      - Worst fit: Search the biggest hole.
      - Quick fit: Maintain a list and find the suitable hole.

## Virtual Memory
1. Pages: Chunks (P195)
   - The virutal address space consists of fixed-size units called pages.
   - The corresponding units in the physical memory are called page frames.
2. Page Tables: Translate the VA into real PA.
   - Translation Lookaside Buffer(TLB): Cache for page table。
   - TLB can be managed by software.
     - Hard Miss: The page is not in the memory and not in TLB. => Major Page Fault
     - Soft Miss: The page is not in the TLB but in the memory(in page tables). => Minor Page Fault
     - Page Table Walk: Looking up the mapping in the page table hierarchy.
   - Segmentation Fault: The program access the invalid address.
3. Multilevel Page Tables:
   - Take the page table into page. 10 - PT1, 10 - PT2, 12 - Offset.
   - Store page table in the disk. Not all the page should put into the memory.
   - Page Table can cause a page fault.
4. Inverted Page Tables:
   - Record the real physical address.
   - Can be very slow since the address is sorted by physical address.
   - Usually use hash to implement high speed query.

## Page Replacement Algorithms
1. Not Recently Used Page Replacement
2. FIFO
3. Second-Chance Page Replacement
4. Clock Page Replacement
5. LRU
6. Working set: When use, when swap
7. WSClock: Like Second-Change Page Replacement, but do replace in clock.

## Segmentation
Stack? Data?