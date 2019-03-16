## Operating System (OS)

**Resource allocation**

* Policy: Manages all resources
* Decides between conflicting requests (efficiency and fairness)

**Control program**

* Mechanism: Controls execution of programs (prevent errors and misuse)
* Modern operating system is interrupt driven. The operating system preserves the state of the CPU by storing registers and the program counter

#### Kernel

* runs with special privileges – it can do what normal user code cannot do (e.g., access to special instructions & special memory regions)
* Hardware support required – CPU has (at least) dual mode operation: user (unprivileged) mode vs. kernel (privileged) mode
* System calls let (unprivileged, untrusted) user programs access (privileged, trusted) kernel services
* System call changes mode to kernel, return from call (via return-from-trap or RETT instruction) resets it to user

| Kernel type  | Idea      | Benefits         | Detriments         |
| ------------ | --------- |----------------- | ------------------ |
| Monolithic   | Simple layered structure | ? | ? |
| Microkernel  | Move services into user space | Easier development, more reliable | Performance overhead of communication between user and kernel space |
| Hybrid       | ? | ? | ? |

```
TODO
Overview of kernel types
```

**Dual-mode Operation**

* Software error or request creates exception or trap (software interrupt)
* Dual-mode operation: User mode and (privileged) kernel mode
* Mode bit provided by hardware provides ability to distinguish when system is running user code or kernel code
* System program is in user mode!
* System call: API to services provided by OS kernel

```
TODO
note on priviledge rings
```

**OS Services**

* Communications between processes (IPC)
* Error and misuse detection: Owners of information stored in a multiuser or networked computer system may want to control use of that information; concurrent processes should not interfere (logically) with each other

#### Computer-system operation

* One or more CPUs, device controllers connect through common bus providing access to shared memory and other resources

**Processes**

* Process is a program in execution. It is a unit of work within the system. Program is a passive entity, process is an active entity which requires resources to complete a task
* Process contains a PC, stack, data section and allocated memory in heap
* Process defines a private address space
* Process couples abstractions such as concurrency and protection
* Process identified and managed via `int pid` process identifier
* Single-threaded process has one program counter specifying location of next instruction to execute, while multi-threaded process has one program counter per thread

| Process State   | Explanation           |
| --------------- | --------------------- |
| new             | being created         |
| running         | executing on CPU      |
| waiting/blocked | wait for I/O or event |
| ready           | queue for execution   |
| terminated/stop | finished execution    |

Process control block (PCB) is a data structure that contains all information about active processes.

```
TODO
queues
```

* Job queue: set of all processes
* Ready queue: processes in RAM that are ready to execute
* Device queue: waiting for I/O

**Multi-process synchronization**

* Cooperating processes may affect each other, mainly through sharing data
* Two basic models of IPC: Shared memory and Message passing
* Socket as an example of message passing
* Thread: line of execution, has registers + stack
* Many threads can run within a process, sharing the address space

* Kernel Thread
  * known to OS, scheduled by kernel CPU, expensive
* User Thread
  * not known to OS kernel, scheduled by user-mode thread scheduler (`pthread`), less expensive

```
TODO
mapping
```

Producer-Consumer Problem

* producer puts a new item of work into a shared buffer
* consumer takes an item of work from the buffer
* buffer can store a fixed number of items (bounded buffer)

Critical Section Problem

* Mutual Exclusion - If process is executing in its critical section (CS), then no other processes can be executing in their critical sections
* Progress - If no process is executing in its critical section and there exist some processes that wish to enter their critical section, then the selection of the processes that will enter the critical section next cannot be postponed indefinitely
* Bounded Waiting - A bound must exist on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted

Peterson's Solution

* require busy waiting, generally bad for uniprocessors
* busy waiting (spinlock) means executing instructions without doing anything useful

```
TODO
```

Synchronization Hardware

* Many modern machines provide special atomic hardware instructions
* Test original value of memory word and set its value
* Swap two memory words

Semaphore

* Semaphore is a high-level synchronization primitive
* No busy waiting
* Two atomic operations on the `int` **state variable**
* `acquire()` ++ and `release()`
* These are critical sections, hence require hardware instructions or use of spinlock
* Binary (0/1) semaphore or Counting semaphore (`int`)

```
TODO
mutex
```

Java Synchronization

* Each Java object has an associated binary lock
* Lock is acquired/released by invoking a synchronized method
* Mutual exclusion is guaranteed for this object’s method – at most only one thread can be inside it at any time
* Threads waiting to acquire the object lock are placed in the **entry set** for the object lock


**Single-core**

* Concurrency: CPU performs context switching so frequently that it gives the illusion of multi-tasking and users can interact with each job while it is running

**Multi-core**

```
TODO
```

**Multi-processor**

```
TODO
```

* Multiprocessing overheads like context switching, IPC, or inter-process synchronization. Hence the speed-up in reality will never be the theoretical maximum of `N`
* Context switching in user threads us faster because it doesn’t have to trap to the kernel (no system call overhead)

Process control block

**Distributed Systems**

```
TODO
```

#### Interrupt Handling

* Interrupt handling is done in **kernel mode**, by the service routine
* Interrupts transfers control to the interrupt service routine generally, through the **interrupt vector**, which contains the addresses of all the service routines
* Interrupt architecture must save the address of the interrupted instruction. Incoming interrupts are disabled while another interrupt (of same or higher priority) is being processed to prevent a lost interrupt or reentrancy problems
* **Context switching**: need to save/restore into PCB all the CPU hardware state that keeps track of the process’s execution, e.g., registers like SP, PC, PSR, etc. to/from main memory
* OS Scheduler interrupts periodically running processes give control to the OS, which can then force rescheduling even if the process doesn’t give up the CPU voluntarily
* **trap** is software-generated interrupt caused either by an error or a user request to allow a user program to invoke an OS function (system call) and run it in kernel mode

* Polling interrupt checks each I/O devices one by oneon whether this is the correct device that requests for I/O interrupt
* Vectored interrupt identifies the device that sent the request

#### Storage Hierarchy

| System     | Cost    | Speed   | Capacity |
| ---------- | ------- | ------- | -------- |
| Registers  | highest | highest | < KB     |
| L(X) Cache |         |         | > MB     |
| RAM        |         |         | > GB     |
| NVRAM      |         |         | > GB     |
| Disk       | lowest  | slowest | > TB     |

* Caching: Information in use copied from slower to faster storage temporarily
* If processes don’t fit in memory, swapping moves them in and out to run

```
TODO:
Caching algorithms

Note on NVRAM
```

```
done up to week 4
```
