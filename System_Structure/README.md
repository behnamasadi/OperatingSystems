# Microkernel

# Monolithic Kernel

# Kernel mode and User mode of CPU operation

# System Call
A system call is a way for programs to interact with the operating system. A system call is the programmatic way in which a computer program requests a service 
from the kernel of the operating system it is executed on.
## Types of System Calls 
There are 5 different categories of system calls –

- Process control: end, abort, create, terminate, allocate and free memory.
- File management: create, open, close, delete, read file etc.
- Device management
- Information maintenance
- Communication


|                      | WINDOWS                 |  UNIX   |
|------------------    |---                      |---------|
|**Process Control**   |CreateProcess()          |fork()   |
|                      |ExitProcess()            |exit()   |
|                      |WaitForSingleObject()    |wait()   |
|**File Manipulation** |CreateFile()             |open()   |
|                      |ReadFile()               |read()   |
|                      |WriteFile()              |write()   |    
|                      |CloseHandle()            |close()   |    
|**Device Manipulation**|SetConsoleMode()               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    
|                      |               |   |    



Refs [1](https://www.geeksforgeeks.org/introduction-of-system-call/)