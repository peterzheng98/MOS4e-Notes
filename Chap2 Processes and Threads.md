# Chapter 2 Processes and Threads

## Process
1. Process Creation:
   - System Initialization
   - Execution of a process-creation system call by a running process
   - A user request to create a new process
   - Initiation of a batch job
2. Daemon Process: Stay in the background to handle some activity such as email, web, news and so on.(P89)
3. Process Termination:(P90)
   - Normal Exit(voluntary) exit code = 0
   - Error Exit(voluntary) exit code != 0
   - Fatal Error(involuntary) runtime error
   - Killed by another process(involuntary) SIGKILL, SIGTERM
4. Process States: Running, Ready, Blocked
5. Process Table:
   |Process Management|Memory Management|File Management|
   |------------------|-----------------|---------------|
   |Registers|Pointer to text segment|Root Directory|
   |Program Counter|Pointer to data segment|Working Directory|
   |Program status word|Point to text segment|File descriptors|
   |Stack pointer||UserID|
   |Process State||GroupID|
   |Priority|
   |Scheduling parameters|
   |Process ID|
   |Parent process|
   |Process Group|
   |Signals|
   |Time when process started|
   |CPU time used|
   |Children's CPU time|
   |Time of next alarm|
6. About Multiprogramming
   - CPU Utilization: Spend a fraction $p$ of its time for I/O. Then CPU Utilization can be written as $1-p^n$ where there are $n$ processes in memory at once.(P96)
 
## Thread
1. Usage:
   1. For the requirements that multiple activities are going at once.
   2. Have lighter weight than processes and easier to create and destory.
   3. Performance arguments. (P98, Threads yield no performance gain when all of them are CPU Bound)
   4. Useful on systems with multiple CPUs.
2. Sharing items in thread
   |Per-process items|Per-thread items|
   |-----------------|----------------|
   |Address Space|Program Counter|
   |Global Variables|Registers|
   |Open files|Stack|
   |Child Processes|State|
   |Pending alarms|
   |**Signal and signal handlers**|
   |Accounting Information|
3. Thread in User Space
   - Advantages: 
     - User-level threads package can be implemented on an OS that does not support threads.
     - Allow each process to have its own scheduling algorithm.
   - Private thread table: Program counter, stack pointer, registers, state...
   - Problem: How to block system calls? How to cope with Interrupt?
4. Thread in Kernel:
   - Set up an kernel thread table
5. Hybrid Implementation: Kernel Thread + Multiple user threads on a kernel thread
6. Pop-up thread: The arrival of a message causes the system to create a new thread to handle.

## InterProcess Communication(IPC)
1. Three basic concepts:
   1. How one process can pass information to another.
   2. Make sure two or more processes do not get in each other's way.
   3. Proper sequence for two process. (P119)
2. Race conditions: Two or more processes are reading or writing some shared data and the final result depends on who runs precisely when.
   - How to avoid? Find some way to prohibit more than one process from reading and writing in the shared data at the same time. (c.f. mutex)
3. Critial Regions: Some part of program has to access shared memory or files or do other critical things that can lead to races.
   - Four conditions to solve the problem:
     - No two processes may be simultaneously inside their critical regions.
     - No assumptions may be made about speeds or the number of CPUs.
     - No process running outside its critical region may block any process.
     - No process should have to wait forever to enter its critical region.
4. Mutual exclusion with busy waiting: (互斥锁, P122)
   - Abstraction:
     - Prevent two process to read and write on the shared resources at the same time.
     - Split the code into critical sections.
   - Disabling Interrupts:
     - Advantage: The simplest solution
     - Disadvantage: Only apply for single CPU processor, user program should not disable interrupt.
   - Lock Variables:
     - Use a shared lock variable.
     - The lock itself will cause race problem.
   - Strict Alternation:
     - **Busy waiting**: Continue testing a variable until some value appears.
     - **Spin lock**: The lock using busy waiting. It is fast if the wait is short but waste CPU cycles if not.(P134)
     - **Peterson's Algorithm**: Book P125
     - **TSL Instruction**: Hardware instruction solution
   - The Producer-Consumer Problem:
     - The producer should not put data in the full buffer.
     - The consumer should not read data in the empty buffer.
     - Bad algorithm may lead to race condition.
5. Semaphores: (信号量, P130)
   - Semaphore is simply a variable that is non-negative and shared between threads.
   - Before enter into the critical region, the semaphore should be lowered and if the semaphore is smaller than 0 then the process will be blocked.
   - After exit from the critical region, the semaphore will be increased.
   - Binary semaphore is mutex.
   - Another important application is **synchronization**（同步）.
6. Mutex: (P132)
   - A shared variable that can be in one of two states: unlocked or locked.
   - Binary semaphore
   - futexes: Fast user space mutex. 
     - Consist of two part: Kernel service and a user library.
     - Atomic decrement and test.
     - Work completely in user space
     - Do something special to maintain the program in user mode.
       - `Down()` operation: if the variable decreases to 0, then no need for turning into kernel mode.(Decrease to 0 means no race happened.)
       - `Up()` operation: if the variable increases to 1, then no need for turning into kernel mode.(No race happened.)
   - Condition Variables: allow threads to block due to some condition not being met.
     - Unlike semaphores, condition variables have no memory.
     - **Potential data loss**: If a signal is sent to a condition variable on which no thread is waiting, the signal will lost.
     - Control by signals.
7. Monitors: (管程, P138)
   - A collection of procedures, variables and data structures that are all grouped together in a special kind of module or package.
   - A monitor is a synchronization construct that allows threads to have both mutual exclusion and the ability to wait (block) for a certain condition to become false.
   - Thread safe interface.
8. Message Passing: Authentication and acknowledgement mechanism.
9. Barriers: Allow groups of processes to synchronize.
10. Avoid Locks: RCU(Read-Copy-Update):
    - In Linux Implementation: By memory barrier.
    - ??

## Scheduling
1. Scheduling Algorithm Goal:
   1. All Systems:
      - Fairness: giving each process a fair share of the CPU
      - Policy enforcement: seeing that stated policy is carried out
      - Balance: keeping all parts of the system busy
   2. Batch Systems:
      - Throughput: maximize jobs per hour
      - Turnaround Time: minimize time between submission and termination
      - CPU Utilization: keep the CPU busy all the time
   3. Interactive Systems:
      - Response time: respond to requests quickly
      - Proportionality: meet users' expectations
   4. Real-time systems
      - Meeting deadlines: avoid losing data
      - Predictability: avoid quality degradation in multimedia systems
2. First-Come, First-Served(Batch System)
3. Shortest Job First(Batch System)
4. Shortest Remaining Time Next(Batch System)
5. Round-Robin
6. Priority Scheduling

## Classical IPC Problems
1. The dining philosophers problem
   - Due to deadlock and not release the resource
   - Not enough resource
2. The readers and writers problem
   - A file with multiple readers and writers.
   - Read First or Write First?