# Process and CPU Scheduling

A process is a program in execution. A process is an ‘active’ entity, as opposed to a program, which is considered to be a ‘passive’ entity. 

## Multiprogramming 
We have many processes ready to run. There are two types of multiprogramming:
## Pre-emption
Process is forcefully removed from CPU. Pre-emption is also called as time sharing or multitasking.
### Non pre-emption
Processes are not removed until they complete the execution.

### Degree of multiprogramming –
The number of processes that can reside in the ready state at maximum decides the degree of multiprogramming, e.g., if the degree of programming = 100, this means 100 processes can reside in the ready state at maximum.

## Memory Layout of C Programs (A Running Process)

 1)Text  
     code and data (variable, functions parameters,...) are stored in separate memory. All codes in the functions
     go into the code (Text) region.
 2)Data  
    global/static initialized data
 3)BSS  
    global/static uninitialized data
 4) Heap
 5) Stack
 It is a dynamic data structure maintained by OS to control the way procedure calling each other and parameter they pass.
 Call stack maintained for each thread. The actual implementation of a stack depends on the microprocessor architecture.
 It can grow up or down in memory and can move either before or after the push/pop operations
 stack is used for functions. It is on top of the memory and moves downward. each function will get its own frame.
 Each instance of the function has its own frame. Data that go into frame are:
 1) Arguments
 2) Local variables (object, data structure)
 3) Saved registers.
 In high level languages stack will be managed automatically. In low level languages
 it is being done with stack pointer and  frame pointer.
 Stack pointer:It tell us where is the boundary between allocated and unallocated memory is.
 Frame pointer: is a reference pointer allowing us to know where local variable or an argument starts.
```
 ---------------------------------
 |   command line arguments      |
 |   argv[0], argv[1] ...argv[n] |
 ---------------------------------
 |       Allocated memory        |
 |-------------------------------|<--- frame pointer      ┐
 |        Return Address         |                        |
 |          Arguments            |                        |  stack frame1
 |        Local Variables        |                        |
 |        Saved registers        |                        |
 |-------------------------------|<---- stack pointer     ┘
 |                               |
 |                               |  <-stack frame2
 |-------------------------------|
 |                               |
 |                               |  <-stack frame3
 |-------------------------------|
 |                               |
 |                               |
 |                               |
 |                               |
 |                               |
 |                               |
 |              HEAP             |
 |                               |
 |-------------------------------|            ┐
 |  global/static uninitialized  |            |
 |             data              |            |<- BSS
 |                               |            |
 |-------------------------------|            ┘  ┐
 | global/static initialized data|               |<-  Data
 |-------------------------------|               ┘
 |          Code(Text)           |
 |                               | <- Program counter point to line in this region
 |                               |
 ---------------------------------
 |          Reserved             |
 |                               |
 ---------------------------------
```

The `size` command reports the sizes (in bytes) of the Text, Data, and BSS segments.

```
int main(void)
{
    return 0;
}
```
Output of size:

```
text       data        bss
960        248          8
```

If we add some global variables:

```
int global_var;
int main(void)
{
    return 0;
}
```
Output of size: (because of "global_var" 4 bytes added to bss)
```
text       data        bss
 960        248         12
```

If we add static variables:
```
int global_var;
int main(void)
{
    static int i;
    return 0;
}
````
Output of size: (because of "i" 4 bytes added to bss)
```
text       data        bss
 960        248         16
```

If we set some values for our static var `i`:

``` 
int main(void)
{
    static int i = 100;
    return 0;
}
```
Output of size: (because "i" initialized 4 bytes added removed from bss and added to data)
```
text       data        bss
 960        252         12
```

### Process Control Block (PCB)
A Process Control Block is a data structure maintained by the Operating System for every process. 
To identify the processes, it assigns a process identification number (PID) to each process. As the operating system 
supports multi-programming, it needs to keep track of all the processes. For this task, the `process control block (PCB)` is 
used to track the process’s execution status. Each block of memory contains information about the process state, program counter, 
stack pointer, status of opened files, scheduling algorithms, etc. All these information is required and must be saved when the process is switched from one state to another. When the process makes a transition from one state to another, the operating system must update information in the process’s PCB.
When the process makes a transition from one state to another, the operating system updates its information in the process’s PCB. The operating system maintains pointers to each process’s PCB in a process table so that it can access the PCB quickly.
1. Process Id    
A unique identifier assigned by the operating system

2. Process State  
Can be ready, running, etc.

3. CPU registers  
Like the Program Counter (CPU registers must be saved and restored when a process is swapped in and out of CPU)


4. I/O status information  
For example, devices allocated to the process,  open files, etc.

5. CPU scheduling information  
For example, Priority (Different processes may have different priorities, for example a short process may be assigned
 a low priority in the shortest job first scheduling)


## Context Switch
A context switch is the mechanism to store and restore the state or context of a CPU in Process Control block so that a process execution can be resumed from the same point at a later time.

## States of Process and Process Life Cycle
A process is in one of the following states:

### New (Create)
In this step, the process is about to be created but not yet created, it is the program which is present in secondary memory that will be picked up by OS to create the process.

### Start
This is the initial state when a process is first started/created.

### Ready
Ready to run. The process is waiting to be assigned to a processor. Ready processes are waiting to have the processor
allocated to them by the operating system so that they can run. Process may come into this state after
Start state or while running it by but interrupted by the scheduler to assign CPU to some other process.
Processes that are ready for execution by the CPU are maintained in a queue for ready processes.

### Running
Once the process has been assigned to a processor by the OS scheduler, the process state is set to running and the
processor executes its instructions.
### Blocked or wait
Process moves into the waiting state if it needs to wait for a resource, such as waiting for user input,
or waiting for a file to become available.
### Terminated or Exit
Once the process finishes its execution, or it is terminated by the operating system, it is moved to the
terminated state where it waits to be removed from main memory.
Process is killed as well as PCB is deleted.


## Process Schedulers in Operating System

The process scheduling is the activity of the process manager that handles the removal of the running process from the CPU 
and the selection of another process on the basis of a particular strategy.


## Schedulers
Schedulers are special system software which handle process scheduling in various ways. Their main task is to select the jobs to be 
submitted into the system and to decide which process to run. Schedulers are of three types −
1) Long-Term Scheduler
2) Short-Term Scheduler
3) Medium-Term Scheduler

### Long Term Scheduler
It is also called a `job scheduler`. A long-term scheduler determines which programs are admitted to the system for processing. It selects processes from the queue and loads them into memory for execution. Process loads into the memory for CPU scheduling.
It also controls the degree of multiprogramming. If the degree of multiprogramming is stable, then the average rate of process creation must be equal to the average departure rate of processes leaving the system.

### Short Term Scheduler
It is also called as `CPU scheduler`. Its main objective is to increase system performance in accordance with the chosen set of criteria.
 It is the change of ready state to running state of the process. CPU scheduler selects a process among the processes that are ready to execute and allocates CPU to one of them.
Short-term scheduler only selects the process to schedule it doesn’t load the process on running.
Dispatcher is responsible for loading the process selected by Short-term scheduler on the CPU (Ready to Running State) Context switching is done by dispatcher only.

## Medium-Term Scheduler
It is responsible for suspending and resuming the process. It mainly does `swapping` (moving processes from main memory to disk and vice versa).

## Process Privileges

### User mode
When the computer system run user applications like creating a text document or using any application program,
then the system is in the user mode. When the user application requests for a service from the operating system
or an interrupt occurs or system call, then there will be a transition from user to kernel mode to fulfill the requests.
### Kernel Mode
When the system boots, hardware starts in kernel mode and when operating system is loaded, it start user application in user mode.
To provide protection to the hardware, we have privileged instructions which execute only in kernel mode.
If user attempt to run privileged instruction in user mode then it will treat instruction as illegal and traps to OS.
Some of the privileged instructions are:
## Handling Interrupts
To switch from user mode to kernel mode.
Input-Output management.
3) Process ID Unique identification for each of the process in the operating system.
4) Pointer A pointer to parent process.
5) Program Counter: Program Counter is a pointer to the address of the next instruction to be executed for this process.
6) CPU registers: Various CPU registers where process need to be stored for execution for running state.
7) CPU Scheduling Information Process: priority and other scheduling information which is required to schedule the process.
8) Memory management information: This includes the information of page table, memory limits, Segment table depending on memory used by the operating system.
9) Accounting information: This includes the amount of CPU used for process execution, time limits, execution ID etc.
10) IO status information: This includes a list of I/O devices allocated to the process.
CPU Scheduling in Operating Systems
The OS maintains all PCBs in Process Scheduling Queues. The OS maintains a separate queue for each of the process
states and PCBs of all processes in the same execution state are placed in the same queue.
When the state of a process is changed, its PCB is unlinked from its current queue and moved to its new state queue.
The Operating System maintains the following important process scheduling queues −
Job queue − This queue keeps all the processes in the system.
Ready queue − This queue keeps a set of all processes residing in main memory, ready and waiting to execute. A new process is always put in this queue.
Device queues − The processes which are blocked due to unavailability of an I/O device constitute this queue.

Types of schedulers:
Long term
Short term
Medium term
## CPU Scheduling in Operating Systems
Arrival Time: Time at which the process arrives in the ready queue.
Completion Time: Time at which process completes its execution.
Burst Time: Time required by a process for CPU execution.
Turn Around Time: Time Difference between completion time and arrival time.
Turn Around Time = Completion Time – Arrival Time
Waiting Time(W.T): Time Difference between turn around time and burst time.
Waiting Time = Turn Around Time – Burst Time

## Different Scheduling Algorithms
In segmented memory management system scheme, each process is atomic, it can not be split up, it either fully
in the memory or not. Segments are swapped between disc and main memory. Segments vary in size. The OS
knows start and physical addresses of each segment. Segmentation can result in externally memory fragmentation.
Logical Memory, Physical Memory
A program view of the memory is called logical memory. Each program is divided into equal size pages.
Each page might be anywhere in the physical memory or even in the swap. To make it possible, OS maintains a
page table, that maps which logical correspond to which physical location.
If there not enough space on the memory, some pages might be move into the swap.
Paged memory can led to fragmented process which run more slowly, but make better use of space.
Paging may lead to internal fragmentation, segmentation may lead to external fragmentation.
```
 Processes              Logical Memory                            Physical Memory
┌-------------┐         ┌-------------┐                          ┌-------------┐
|  process1   |         |--process1---|                          |--process3---|
└-------------┘         |--process1---|                         /|-------------|
                        |-------------|                        / |-------------|
┌-------------┐         |-------------|     Page Table        /  |-------------|
|  process2   |         |-------------|    ┌---------┐       /   |-------------|           Virtual Memory Swap
|             |         |-------------|    |---------|      /    |-------------|             ┌-------------┐
|             |         |-------------|    |---------|     /     |-------------|             |-------------|
└-------------┘         |-------------|    |---------|    /      |-------------|             |-------------|
                        |-------------|    |---------|   /       |-------------|             |-------------|
┌-------------┐         |-------------|   /|---------|--/--------|-------------|-------------|--process3---|
|             |         |-------------|  //|---------|\/         |-------------|             |-------------|
|             |         |-------------| ///|---------|/ \        |-------------|             └------------ ┘
|  process3   |         |--process3---|////|---------|----\------|--process3---|
└-------------┘         |--process3---|/// |---------|      \    |-------------|
                        |--process3---|//  |---------|        \  |-------------|
┌-------------┐         |--process3---|/   └---------┘          \|--process3---|
|  process4   |         └-------------┘                          └------------ ┘
└-------------┘
``` 
In this scheme, the operating system retrieves data from secondary storage (disk) in same-size blocks called pages
 Paging is a memory management scheme that allows a process to be stored in a memory in a non-contiguous manner.
 Storing process in a non-contiguous manner solves the problem of external fragmentation.
 For implementing paging the physical and logical memory spaces are divided into the same fixed-sized blocks.
 These fixed-sized blocks of physical memory are called frames, and the fixed-sized blocks of logical memory are called pages.
 List of Process Attributes:
 1)Process ID
 2)Process Counter: after resuming the process, from which line should we continue
 3)Process State:
 4)
 5)
 6)
 7)
 8)



## Process Life Cycle/ State

### Start
This is the initial state when a process is first started/created.

### Ready
The process is waiting to be assigned to a processor. Ready processes are waiting to have the processor 
allocated to them by the operating system so that they can run. Process may come into this state after
Start state or while running it by but interrupted by the scheduler to assign CPU to some other process.


### Running
Once the process has been assigned to a processor by the OS scheduler, the process state is set to running and the
processor executes its instructions.


### Waiting
Process moves into the waiting state if it needs to wait for a resource, such as waiting for user input,
or waiting for a file to become available.


### Terminated or Exit
Once the process finishes its execution, or it is terminated by the operating system, it is moved to the 
terminated state where it waits to be removed from main memory.






## CPU Scheduling in Operating Systems
The OS maintains all PCBs in Process Scheduling Queues. The OS maintains a separate queue for each of the process
states and PCBs of all processes in the same execution state are placed in the same queue.
When the state of a process is changed, its PCB is unlinked from its current queue and moved to its new state queue.

The Operating System maintains the following important process scheduling queues −

Job queue − This queue keeps all the processes in the system.

Ready queue − This queue keeps a set of all processes residing in main memory, ready and waiting to execute. A new process is always put in this queue.

Device queues − The processes which are blocked due to unavailability of an I/O device constitute this queue.






### Different Scheduling Algorithms


In segmented memory management system scheme, each process is atomic, it can not be split up, it either fully 
in the memory or not. Segments are swapped between disc and main memory. Segments vary in size. The OS 
knows start and physical addresses of each segment. Segmentation can result in externally memory fragmentation.

Logical Memory, Physical Memory
A program view of the memory is called logical memory. Each program is divided into equal size pages.
Each page might be anywhere in the physical memory or even in the swap. To make it possible, OS maintains a 
page table, that maps which logical correspond to which physical location.
If there not enough space on the memory, some pages might be move into the swap.
Paged memory can led to fragmented process which run more slowly, but make better use of space.
Paging may lead to internal fragmentation, segmentation may lead to external fragmentation.




In this scheme, the operating system retrieves data from secondary storage (disk) in same-size blocks called pages


Paging is a memory management scheme that allows a process to be stored in a memory in a non-contiguous manner. 
Storing process in a non-contiguous manner solves the problem of external fragmentation.

For implementing paging the physical and logical memory spaces are divided into the same fixed-sized blocks. 
These fixed-sized blocks of physical memory are called frames, and the fixed-sized blocks of logical memory are called pages.








Ref [1](https://www.tutorialspoint.com/operating_system/os_process_scheduling.htm),
    [2](https://www.geeksforgeeks.org/process-schedulers-in-operating-system/),
    [3](https://www.youtube.com/watch?v=qlH4-oHnBb8),
    [4](https://www.youtube.com/watch?v=dz9Tk6KCMlQ),
    [5](https://www.youtube.com/watch?v=xD5PB_g1rIE),
    [6](https://en.wikipedia.org/wiki/Paging#Ferranti_Atlas),
    [7](https://en.wikipedia.org/wiki/Virtual_address_space),
    [8](https://en.wikipedia.org/wiki/Page_(computer_memory)),
    [9](https://en.wikipedia.org/wiki/Page_replacement_algorithm)
