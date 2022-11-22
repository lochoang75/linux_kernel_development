# Linux kernel deveopment, 3rd edition
## Introduction to the linux kernel
- The original elegant design of the Unix system, along with the years of innovation and evolutionary improvement that followed, has resulted in a powerful, robust, and stable operating system.
- First, **Unix is simple**: Whereas some operating systems implement thousands of system calls and have unclear design goals, Unix systems implement only hundreds of system calls and have a straightforward, even basic, design.
- Seccond, in Unix, *everything is a files*, this simplifies the manipulation of data and devices into a set of core system calls: **open()**, **read()**, **write()**, **lseek()** and **close()**
- Third, the Unix kernel and related system utilities are written in C - A property that gives Unix its amazing portability to diverse hardware architechtures and accessibility to a wide range of developers.
- Fourth, Unix has fast process creation time and the uinque **fork()** system call.
- Finally, Unix provides simple yet robust interprocess communication(IPC) primitives that, when coupled with the fast process creation time, enable the creation of simple program that *do one thing and do it well*
## Linux vs Classic Unix
- a Unix kernel is typically a monolithic static binary.
- a Unix system typically require a system with a paged memory-management unit (MMU).
- We can devide kernels into two main schools of design: the monolithic kernel and the micro kernel.
- Monolithic kernels are the simpler design of the two, and all kernels were designed in this manner util 1980s. Monolithic kernel are implemented entirely as a single process running in a single address space. All kernel services exist and execute in the large kernel address space. Commnunication within the kernel is trivial because everything runs in kernel mode in the same address space: **The kernel can invoke functions directly, as a user-space applicatin might. Proponents of this model cite the simplicity and performance of the monolithic approach.
- Micro kernels, on the other hand, are not implemented as a single large process. Instead, the functionality of the kernel is broken down into separate processes, usually called servers. Ideally, only the servers absolutely requiring such capabilities run in a privileged execution mode.
- The rest of the servers restof the servers run in user-space. All the servers, though, are separted into different address spaces. Therefore, direct function invocation as monolithic kernels is not possible. Instead, micokernels communicate via message passing: An interprocess communication(IPC).
- Linux is a monolithic kernel; that is, the Linux kernel executes in a single address space entirely in kernel mode. Linux, however, borrows much of the good from microkernels: Linux boasts a modular design, the capability to preempt itselft(called *kernel preemption*), support for kernel threads, and the capability to dynamically load separate binaries (kernel modules) into the kernel image.
- Everything run in kernel mode, with direct function invocation not message passing, the modus of communication. Nonetheless, Linux is modular, threaded, and the kernel itself is schedulable.
## A Beast of a different nature
- Some differences between kernel development and user-space development:
    + The kernel has access to neither the C library nor the standard C headers.
    + The kernel is coded in GNU C.
    + The kernel lacks the memory protection afforded to user-space.
    + The kernel cannot easily execute floating-point operations.
    + The kernel has a small per-process fixed-size stack.
    + Because the kernel has asynchronous interrupts, is preemptive, and supports SMP, synchronization and concurrency are major concerns within the kernel.
    + Portability is important.
- Branch Annotation
    + The gcc C compiler has a built-in directie that optimizes conditional branches as either very likely taken or very unlikely taken. The compiler uses the directive to appropriately optimize the branch.
    + The kernel wraps the directive in easy-to-use macros, *likely()* and *unlikely()*
    + These directives result in a performance boost when the branch is correctly marked, but a performance *loss* when the branch is mismarked. A common usage for *unlikely()* and *likely()* is error conditions.
- No memory protection
    + Memory violations in the kernel result an *oops*, which is a major kernel error.
    + Additionally, kernel memory is not pageable. Therefor, every byte of memory you consume is one less byte of available physical memory.
- Small, Fixed-Size Stack
    + The kernel stack is two pages which generally implies that it is 8KB on 32 bit architechtures and 16KB on 64 bit architechtures. This size is fixed and absolute.
- Synchronization and Concurency
    + Linux is a preemptive multitasking operating system. Processes are scheduled and rescheduled at the whim of the kernel's process scheduler. The kernel must synchronize between these tasks.
    + Linux sypports symmetrical multiprocessing(SMP). Therefore, without proper protection, kernel code executing simutaneously on two or more processors a=can control-currently access the same resource.