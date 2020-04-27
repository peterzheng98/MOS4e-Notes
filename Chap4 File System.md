# Chapter 4 File System

## Directories
1. Absolute File Path: The path from root to the file.

## File System Implementation
1. MBR: Master Boot Record, Sector 0 of the disk.
2. Implementing Files:
   1. Contiguous Allocation: May cause block and holes, UDF.
   2. Linked List Allocation
   3. Linked-List Allocation using a table in memory: FAT
   4. I-Node(Index-Node)