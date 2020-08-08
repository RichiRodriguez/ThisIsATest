## Tips for Completing Project 2

### Issues to Consider

Here is an outline of the the major issues you will have to deal with to make Nachos into a multiprogrammed system:

* In order to handle the various system calls, you will need to modify the **`ExceptionHandler()`** function in **`exception.cc`** to determine which system call or exception occurred, and to transfer control to an appropriate function. You might want to consider introducing "stubs" (functions with empty bodies or with debugging printout so you can tell when they are called) for all the system calls right away, and then postpone their actual implementation until a bit later. This strategy will help you understand better how control is transferred from user mode to system mode when a system call is executed.
* The Nachos code you have been given is extremely simple-minded about memory management. In particular, the constructor function **`AddrSpace::AddrSpace()`** simply determines the amount of memory that will be required by the application to be run and then allocates that much space contiguously starting at address zero in physical memory. The page tables (which control the address translation hardware) are set up so that the logical addresses (what the user program sees) are identical to the physical addresses (where the data is actually stored). The above scheme is inadequate for running more than one application at a time. You will need to design and implement a scheme for allocating and freeing physical memory, and you will need to arrange to set up the page tables so that the logical address space seen by a user  application is a contiguous region starting from address zero, even though the data is stored at different physical addresses. You will want to implement a memory management scheme that is flexible enough to extend to virtual memory later in the semester. We suggest implementing a C++ class with methods for allocating and freeing physical memory one page at a time. By setting up the page tables properly, you can give the user application a contiguous logical address space even though each page of actual data might be stored anywhere in physical memory.
* The **`Fork()`** system call is the most difficult part of this assignment. It is different from the system call Exec in that Fork will start a new process that runs a user function specified by the argument of the call, while Exec will start a process that runs a different executable file. The parameter types for **`Fork()`** and **`Exec()`** also differ. **`Fork(func)`** takes an argument **`func`** which is a pointer to a function. The function must be compiled as part of the user program that is currently running. By making this system call **`Fork(func)`** , the user program expects the following: a new thread will be generated for use by the user program; and this thread will run func in an address space that is an exact copy of the current one. This implementation of Fork makes it possible to have and to access multiple entry points in an executable file. To make the system call **`Fork(func)`** work for the user program, you will need to know how to find the entry point of the function that is passed as the parameter. The parameter convention is determined by the cross-compiler which produces executable code from the user source program. Look at the file exception.cc to see that this entry point, which is an address in the executable code's address space, is already loaded into register 4 when the trap to the exception handler occurs. All you need to do is to insert code into the exception handler (or call a new function of your own) which does the following: set up an address space which is a copy of the address space of the current thread, and load the address that is in register 4 into the program counter. After these steps, use Thread::Fork() to create a new thread, initialize the MIPS registers for the new process, and have both the new and old processes return to user mode. The parent should return to user mode by returning from the exception handler, the child process should continue to run from the address that is now in the program counter, which is the entry point of the function. To implement Fork, you will need to introduce modifications to the **`AddrSpace`** class in **`addrspace.cc`** so that you can make a "clone" of a running user application program. We suggest adding a function **`AddrSpace::Fork()`**. In brief, calling this function will create a new address space that is an exact copy of the original. You will have to allocate additional physical memory for this copy, set up the page tables properly for the new address space, and copy the data from the old address space to the new. Once the physical memory has been allocated and the page tables set up, you will use **`Thread::Fork()`** to create a new kernel thread, initialize the MIPS registers for the new process, and then have both the old and the new processes return to user mode. The child process should continue by finishing the **`Fork()`** system call. The parent should return to user mode merely by returning from the **`ExceptionHandler()`** function. 
* The **`Exit()`** system call should work by calling **`Thread::Finish()`**, but only after deallocating any physical memory and other resources that are assigned to the thread that is exiting.
* In order to implement the Exec() system call, you will need a method for transferring data (the name of the executable, supplied as the argument to the system call) between the user address space and the kernel. You are not supposed to use functions **`Machine::ReadMem()`** and **`Machine::WriteMem()`** in **`machine/translate.cc`** because they will be needed later. Instead, you will have to code your own functions that take into account the address translations described by the page tables to locate the proper physical address for any given logical address. (Recall that strings in C are stored as sequences of characters in successive memory locations, terminated by a null character.) 
  Once the name of the executable has been copied into the kernel, and the file has been verified to exist, the executable file should be consulted to determine the amount of physical memory required for the new program. This physical memory should be allocated and initialized with data from the executable file, the page tables thread should be adjusted for the new program, the MIPS registers should be reinitialized for starting at the beginning of the new program, and control should return to user mode. File progtest.cc contains a sample for executing a binary program.
  **<u>NOTE</u>**: The object code produced by the MIPS cross-compiler assumes that the data segment begins at the physical address immediately following the text segment. In particular, there is no page alignment, so that if the text segment ends in the middle of a page, then the data segment will start just after it and the page will contain both code and data.
* The **`Yield()`** system call will call **`Thread::Yield()`** after making sure to save any necessary state information about the currently executing process.
* Be sure to synchronize your code correctly. You will need to put lock operations in your code to ensure that it will work properly. Your locking should be fine grained enough to eliminate any spurious latency problems caused by coarse grained locking.

## Suggestions from previous classes

* *Error handling*: System calls invoked by a user process in user mode should never "crash" Nachos. This means you should never trust that the arguments supplied for a system call are correct or reasonable. Instead, these arguments should be checked, and if they are incorrect, a failure status should be passed back to the caller. You will want to think of some scheme for accomplishing this.
* *Build and test incrementally*: We suggest not trying to implement everything at once, but rather do a little at a time, testing that each bit you do works before going on to the next modification. This incremental approach seems to work best for operating systems, since it enables you never to be too far from having a version that "runs''. If you make too many changes all at once, the debugging task to get back to a running system becomes enormous.
* *Adding source files*: Don't be afraid to add new source files to Nachos, especially if it makes your program more modular. For example, it might be reasonable to add a new source file for the physical memory manager.
* *Debugging flags and switches*: Using the "**`-s`**" flag to Nachos along with the "**`-x`**" flag causes Nachos to single-step while in user mode. This might be helpful for debugging and understanding. Also, have a look at the file **`threads/utility.h`** to see all the code letters that can be supplied along with the "**`-d`**" flag to enable various kinds of debugging printout from Nachos. The "**`-d m`**'' option prints out each MIPS instruction as it is executed, which is very helpful for tracing problems with **`Fork()`** and **`Exec()`**.
* *Incrementing Program Counter after a System Call*: Before returning to user mode after a system call, it is necessary to increment the MIPS program counter to point to the next instruction. A system call instruction takes 4 bytes, so you must add 4 to the program counter. If you don't do this, the application making the system call will go into a loop in which it repeatedly makes the same call over and over. This is hard to figure out otherwise, so I'm telling you now so you don't have to.




