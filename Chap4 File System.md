# Chapter 4 File System

## Directories
1. Absolute File Path: The path from root to the file.

## File System Implementation
1. MBR: Master Boot Record, Sector 0 of the disk.
2. Implementing Files:
   1. Contiguous Allocation: May cause block and holes, UDF.
   2. Linked List Allocation
   3. Linked-List Allocation using a table in memory: FAT
   4. I-Node(Index-Node): The file stored in UNIX System is identified by INode Number.
3. Log-Structured File Systems:
   - Same author as Raft.
   - Every file is log and the file will be stored with several versions.
   - Require wrap file if the disk is full.
4. Journaling File Systems:
   - Use the concept of atomic transaction.