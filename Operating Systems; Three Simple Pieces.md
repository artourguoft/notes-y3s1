# <u>2: Introduction</u>
The **operating system** takes physical resources such as the processor, memory, or disk and makes them easy and efficient to use through **virtualization**. Thus, the OS is sometimes referred to as a **virtual machine**.
- Virtualization essentially transforms physical resources into virtual resources, and thereafter exposes a more general, easy to use and easy to protect interface to the underlying physical resources
- Through virtualization, the OS allows many programs to run concurrently, thus sharing CPU, memory, disk, etc. and so the OS is also a **resource manager** 

To expose the user to functionalities such as running a program, accessing a file or allocating memory, the OS has an application programming interface of a few hundred **system calls**; this collection of system calls is often referred to as a **standard library**.
## 2.1: Virtualizing the CPU
Virtualizing the CPU allows us to run programs concurrently; later we will see how this is implemented, what policies are used to determine which program runs when, etc.
- More generally, the OS provides the illusion that each process gets the CPU entirely to itself, as well as the background interface for running and stopping processes, context switching, scheduling, etc.
## 2.2: Virtualizing Memory
Memory is just an array of bytes; to read you must provide an address and amount to read to access the data stored at said address, and to write you must also specify what data to write. 
- Recall that memory stores not only the data used to by programs, but also the series of instructions that constitute the programs themselves
- If the OS virtualizes the CPU and thus allow processes to run concurrently, then it must also create a **virtual address space** for each process which somehow maps to the physical memory without creating conflicts
	- A pointer in one program can hold the same address as a pointer in another program and they will not affect each other, nor affect the OS itself; as far as each process is concerned it has access to the entirety of physical memory to itself
	- In reality, the OS is intricately managing memory and protecting the memory used by each program from others
## 2.3: Concurrency
The OS must also consider several issues that arise when it attempts to facilitate **concurrency**; the completion of multiple tasks in an overlapping time period
- Recall, the CPU executes instructions one at a time, so for example incrementing an integer is not executed **atomically** (all at once) and may take three instructions; one to load the value from memory into a register, one to pass the value thru the ALU, and one to store the result back in memory
- Concurrency is the goal; implementations vary from **context-switching** (between processes or threads, as we will see later) to **parallelism**, etc. 
## 2.4: Persistence
Persistence is when data that survives beyond program execution, reboots, and system failures.
- Memory is **volatile** in that it resets if it loses power or crashes; to persist data we use hard disk drives (HDDs), solid state drives (SSDs), etc., the part of the OS which manages these storage devices is called the **filesystem**
- Storage is **not virtualized** as its very purpose is often to have multiple programs share the saved data, however the OS still does define an easy to use interface to storage
## 2.6: Brief History
Some key developments in OS history to consider:
- Multiprogramming gave rise to the need for concurrency as slow I/O devices would leave the CPU idle while the process waits for device to return data
- Early OSes were largely just libraries of device drivers and commonly used services, later the OS took a more central role for various reasons, ex. the need for privacy for multi-user setups (any given application should not just be able to access any part of storage)
- Hardware itself evolved to have specific instructions and states that made the initialization of the OS itself a more formal and controlled process, and gave rise to the concepts of **procedure calls** vs **system calls**
	- Procedure calls (CPU instructions like JMP, etc.):
		- Run in hardware **user space** which limits access to physical memory, storage, network I/O, etc.
		- Have access only to the process' own **virtualized memory** address space
		- Low overhead, limited only by the speed of the CPU itself and thus very fast
	- System calls (CPU instruction SYSCALL):
		- Run in hardware **kernel space** which allows full access to physical memory, storage, network I/O, etc.
		- Utilize **exceptions** / **interrupts** / **traps** to stop the requesting process, save its state and hand control to the kernel through a **trap handler** (OS code whose location is determined at boot), then upon completion return from trap back to the requestor in user mode
		- Much more overhead than procedure calls, more safety checks and latency from the devices themselves, and thus much slower
# <u>4: Processes</u>
A key abstraction is that of the **process**, which is simply a running program.

To implement virtualization of the CPU, we require low-level methods and protocols called **mechanisms** and high-level algorithms on top of said mechanisms called **policies**
- These mechanisms include **context switches**, and policies include **scheduling policy**, which combined are how the OS facilitates **time sharing**

The abstraction of a running program is called a **process**. Each process has a **machine state**, including (but not limited to):
- **Address space**; the range of memory addresses that the process can access
- Registers; ex. the program counter, stack pointer and frame pointer
- List of **file descriptors**

Consider the basic functions that must be included in an interface of any OS:
- **Create** a process when a program is invoked from the shell or elsewhere
- **Destroy** a process forcefully, as may be necessary for malfunctioning processes; most processes will exit gracefully once they complete their algorithm
- **Wait** for a process to stop running
- **Suspend and resume** a process
- **Status check** on a process
## 4.3: Process Creation
A program is a collection of instructions and static data on disk (stored in some executable format). How does this turn into a process in memory, with said instructions and static data and added memory space for the heap and stack? This is done through **loading** of the program into memory from disk:
- Older and simpler OSes did this **eagerly** meaning all instructions and data were loaded into memory at once, whereas now OSes generally do this **lazily** meaning instructions and data are loaded into memory as they are needed (more on this when we discuss **paging** and swapping with virtual memory)
- Recall, the stack is used for local variables (including function parameters, return addresses); when a process starts up the OS will also likely initialize the provided stack with the values passed into of the `main()` function, namely `argc` and `argv`
- Lastly recall the key differences between stack and heap:
	- The **stack** is **sequential**; it only ever grows or shrinks at the very top (ie. the stack pointer), so the CPU knows exactly where the next variable will be which makes allocation and retrieval incredibly fast - just a single CPU instruction that moves a pointer
		- The stack is attached to the function in context (through a stack frame); preceding functions calls have no way to access the current stack frame, and once the function returns and the stack pointer retreats upward that memory is given up as well (the classic bug of returning a local variable)
	- The **heap** is **scattered**; over time **fragmentation** occurs, with little holes of free memory everywhere - finding and filling these holes is can become algorithmically complex, which is why `malloc()` is significantly slower than stack allocation
		- The heap is not attached to any specific function call, anything that is allocated to the heap from a function will remain throughout the lifetime of the process until it terminates or logic within the process explicitly deallocates that memory (`free()` or a garbage collector, for example)
			- It follows that allocating to the heap makes a program vulnerable to issues like memory leaks and dangling pointers
Finally, once the program is **loaded** and the **stack (and likely heap) is initialized**, the OS transfers control of the CPU to the program by jumping to the `main()` function through a mechanism that we will elucidate in Chapter 5. In Linux, these steps go roughly as follows:
1. Create a PCB (this data is owned and managed by the kernel in its own address space, not in the process' address space itself)
2. Create a new address space (more on this in the section on memory)
3. Initialize the address space with the entire contents of the parent (recall, processes in Linux are created entirely thru `fork()`)
4. Place the PCB in the scheduler's ready queue (more on this in chapters on scheduling)
5. The `fork()` call returns in two processes (the parent and the child)

**Process states** can roughly be categorized as follows, with decisions on how to move processes between these states made by a **scheduler**:
- **Initial / Embryo:** a newly created or currently being created process
- **Running:** the process is currently being executed by the CPU; such a process can be descheduled to the ready state
- **Ready:** the process is ready to run but is not currently being executed by the CPU; such a process can be scheduled to start running
- **Blocked:** the process can continue running pending some external event, ie. the process has requested a read from the disk and is waiting for the data; during this time other processes can utilize the CPU to prevent downtime
- **Final / Zombie:** the process has exited but has not yet been cleaned up by the OS, ie. left in case the parent will call `wait()`

The OS must keep a **process list** to facilitate all the necessary management and scheduling of processes, and each entry in said list must hold key data in a structure often called a **process control block**; below is a non-exhaustive list of what a PCB may contain:
- Process ID (PID)
- Process state
- Data from registers to be saved and then re-loaded into registers upon context switch
- Pointer to start of process memory
- Size of process memory
Note again, the **PCB is stored in the kernel memory reserved for the process**, not in the process' own address space
# <u>7: Scheduling</u>
To compare scheduling policies we need to define **scheduling metrics**. These metrics can be categorized into **performance** and **fairness** metrics, where one usually comes at the expense of the other. For performance, consider:
- **Turnaround Time:** $\text{T}_{\text{turnaround}}=\text{T}_{\text{completion}}-\text{T}_{\text{arrival}}$
	- For a **workload** of jobs, we generally want to consider the **average turnaround time** among all the jobs

First we will assume that all jobs run to completion once started, and that we know the exact amount of time that each job will need. We can build some basic examples of scheduling algorithms from these assumptions:
- **First In First Out (FIFO) / First Come First Serve (FCFS)
	- This algorithm can suffer from the **convoy effect** wherein large jobs that arrive first monopolize the CPU while smaller jobs have to wait, which is obviously detrimental to average turnaround time
- **Shortest Job First (SJF)**
	- This algorithm is optimal and avoids the convoy effect only when **all jobs arrive at the same time** (which is not realistic!); otherwise this algorithm effectively becomes FIFO

So far we have assumed that jobs run to completion once started. Algorithms that adhere to that assumption are called **non-preemptive** and are rarely implemented in modern systems. If we relax this assumption and allow **context-switching** between jobs, we can come up with some **preemptive** algorithms:
- **Preemptive Shortest Job First (PSJF) / Shortest Time to Completion First (STCF)**
	- The scheduler can use the aforementioned timer interrupt flow (or other entry points to the OS, namely new process creation) to preempt a process and run this scheduling algorithm to determine if a context-switch should be executed

Relaxing the assumption on jobs running to completion allows us to consider a fairness metric, which is important for **user-interactive** systems:
- **Response Time:** $\text{T}_{\text{response}}=\text{T}_{\text{firstrun}}-\text{T}_{\text{arrival}}$
	- For a **workload** of jobs, we generally want to consider the **average response time** among all the jobs
	- SJF and PSJF aimed only to reduce average turnaround time, they do not take response time into account at all and in many scenarios would lead to long average response times
		- In fact, **starvation** is a concern for both of these algorithms, wherein shorter jobs keep arriving and being scheduled ahead of a long-waiting large job which is perennially deprioritized
We introduce new scheduling algorithms with the aim of reducing response time:
- **Round Robin (RR)**
	- This algorithm defines a **scheduling quantum** $q$ time slice (which has to be **greater than or equal to the timer interrupt and some multiple of it**) for which it runs each job, and then goes through all jobs in a queue structure
	- It follows that the smaller the scheduling quantum, the faster response time will be, but there is a trade-off
		- As $q\to 0$, the OS overhead time of making scheduling decisions and context switching to dominate overall performance
		- As $q\to \infty$, the response time benefit of running a round robin policy in the first place degrades to effectively being FIFO
	- The optimal scheduling quantum is one that amortizes the overhead of each scheduling decision or context switch over an appropriately long enough interval during which user processes run
	- Round Robin does not take turnaround time into account at all, and thus in many scenarios leads to long average turnaround times
The big **trade-off** so far seems to be between algorithms that prioritize turnaround time and those that prioritize fairness. 

Note, processes are sometimes **blocked** while they wait for external resources, I/O, etc. Then, during such times any preemptive algorithm would preempt that process and run others, until a return interrupt is triggered at which point that process is queued again (how this is done is up to the scheduler as well).

Lastly, all the algorithms we have looked at still have one very unrealistic assumption; that the OS knows how long each process will take a priori. 
# <u>8: The Multi-Level Feedback Queue</u>
The **multi-level feedback queue** attempts to minimize response time for interactive jobs and turnaround time in general, **without a priori knowledge of job length**. It accomplishes this by learning about processes as they run; caching and CPU branch prediction use a similar concept.
- The MLFQ has **several queues**, each with a given **priority level**; ready processes are in a single queue at any point in time
	- Each queue can be configured with its own quantum; higher priority queues are given **shorter** quanta as the jobs in this queue are more likely to be interactive processes and thus need responsiveness, while lower priority queues get **longer** quanta to facilitate fairness
- The **allotment** is a configuration of the MLFQ which determines how long a process can stay in a queue before it is moved to a lower priority queue (which must be greater than or equal to the quantum and preferably some multiple of it)
- New processes are put in the highest priority queue; in this way the MLFQ can approximate SJF for short jobs, ie. a short job that comes in will be given highest priority and may run to completion before it is moved down to low priorities where longer processes may be queued from before
	- Note, the MLFQ generally makes scheduling decisions for such events as well, not just at the current running job's quantum; ie. if a lower priority job is running and a new job enters (at the highest priority) before its full quantum, it will get pre-empted and switched immediately
		- This is in contrast to RR, which is overall a simpler algorithm and thus makes decisions only at the quantum interval
- Processes that relinquish the CPU and become blocked before using up their allotment are kept in their current priority; in this way the MLFQ optimizes for responsiveness of interactive processes
	- The amount of allotment time used is kept accounted for; this prevents processes from **gaming** the MLFQ by always relinquishing the CPU right before allotment is used up
- The MLFQ will **priority boost** all processes to the highest priority after some time period; in this way the MFLQ **prevents starvation** and adjusts accordingly if a process that was **previously not interactive** but has become interactive after some time
	- This time period has to be chosen carefully and is mostly done through trial and error; too long and starvation is not resolved, too short and we approach simple RR
	- This includes currently running or blocked processes; these get their priority level updated immediately and are thus queued in the topmost priority once they move back to ready state

The MLFQ implementation we have defined can be summed up with the rules:
1. If the **priority** of the containing queue of process A is **higher** than that of process B, process A runs **first**
2. If the **priority** of the containing queue of process A is **equal** to that of process B, process A and B run in **RR**
3. When a **new process** enters the scheduler, it is placed in the **highest priority** queue
4. If a process has **used up its allotment** in its current queue and still has not completed, it is immediately (not at the next quantum) moved to the **next lowest priority** queue
5. If a process **becomes blocked**, ie. gives up the CPU before using up its allotment, it will be returned to the **back of the same queue** when it becomes ready again, and allotment usage remains **accounted** for
6. After a certain time period, **priority boost all processes** (including currently running or blocked ones) to the **highest priority**
# <u>9: Proportional Scheduling</u>
**Fair-share** schedulers aim to guarantee that each job obtains a certain percentage of CPU time. 

A basic example is **lottery scheduling**
- **Tickets** are used to represent what percentage of the CPU a process should receive, ie. as a share of the total tickets managed by the system
- The lottery scheduler picks a ticket number at every quantum to determine which process runs; thus, lottery scheduling achieves the desired proportions **probabilistically**
	- This random approach is faster, more lightweight (in terms of state required to track all the information needed by the scheduler), and less prone to edge cases
		- Ie. the scheduler only needs the total number of tickets, the random number, and a data structure that holds the processes and their ticket counts (preferably sorted by said count to optimize traversing for winner)
- To understand fairness in a lottery scheduler, we can consider a simplified model where we have two jobs with equal runtimes, and our **fairness** metric is simply one job's completion time divided by the other's
	- By definition our fairness metric will approach $1$ as the jobs' completion times get closer
	- As we increase job length, fairness also increases as the random selection gets more quanta over which it can spread the processes

Another example of fair-share scheduling is **stride scheduling**
- Unlike lottery, stride scheduling is **deterministic** in achieving the desired proportions
	- The ticket amounts assigned to each process are used as denominators to determine the **stride** as inversely proportional to ticket amount; $\text{stride}=\frac{1}{\text{ticket amt}_{n}}$
		- Each time a process runs we increment its **pass value** by its stride
		- At each quantum, the scheduler runs the process with the **lowest pass value**
- Notice, stride schedulers have to maintain global state (ie. pass values, stride values, determining pass values for new processes to prevent hogging, etc.) unlike lottery schedulers which only maintain total ticket amount

A real world example of a fair-share scheduler is the Linux **Completely Fair Scheduler**
- The CFS uses a counting technique based on the concept of **virtual runtime**
	- In the most simple implementation, `vruntime` accumulates at the same rate as physical time
	- When a scheduling decision occurs, the CFS picks the **lowest** `vruntime`
- The CFS makes scheduling decisions based on a **sched_latency** configuration set in milliseconds, which is then divided by the number of processes to determine the quantum
	- This ensures that over the `sched_latency` time period, the scheduler is essentially a completely fair RR
	- This is dynamic; if jobs complete and no new ones enter, the quantum will increase (by definition of how it is calculated)
	- There is also a **min_granularity** configuration which is the floor on the quantum, which prevents it from becoming too small when the number of processes grows (which would lead to excessive context switching)
		- If this floor is reached, the CFS will not be perfectly fair over the course of the `sched_latency`, but this trade-off must be made
- Speed and efficiency are also important parts of the CFS; one facet of this is the way it finds the next process to run by using the **red-black tree** data structure to store processes (this is key as real systems hold thousands of processes at a time)
	- Regular binary trees can degrade to linear (list-like) performance from some operations; for example:
		- If the processes are sorted by ascending `vruntime`, then retrieval is just $O(1)$ (get the first element), but re-inserting the process back at the end of the quantum is $O(n)$ (might have to check whole list)
	- Red-black trees are always $O(\log n)$
		- These trees are effectively binary search trees with some additional constraints for rebalancing after inserts, etc. and are used in an **intrusive** way by Linux
	- Note, only **running and ready** processes are kept in the tree; thus when new processes or unblocked processes enter, they are given the **minimum** `vruntime` currently in the tree
		- Starting these processes with `vruntime` $0$ or their old value would lead to CPU hogging while they catch up to the `vruntime` amounts already in the system
- There is a mechanism for users to influence whether some jobs get a larger or smaller share of the CPU called the **nice** level of a process
	- For example, nice levels from $[-20,19]$ map to hardcoded **weights** where lower niceness values map to higher values (and thus higher priority) and higher nice levels map to lower values (and thus lower priority)
	- The weights are then used to determine the quantum for a process $k$ of $n$ processes; $\text{quantum}_{k}=\frac{\text{weight}_{k}}{\sum_{i=0}^{n-1}\text{weight}_{i}}\cdot \text{sched\_latency}$; this essentially divides up the `sched_latency` between processes
	- The accounting of `vruntime` must also be scaled; $\text{vruntime}_{k}=\text{vruntime}_{k}+\frac{\text{weight}_{0}}{\text{weight}_{k}}\cdot \text{runtime}_{k}$
	- The hardcoded weights are chosen in such a way that constant differences between niceness values lead to the same proportional difference between quanta and between runtime scaling
# <u>13: Virtual Address Spaces</u>
To time-share memory, the very earliest operating systems simply had only one process in memory at a time, which usually addressed physical memory addresses directly, and on context switches saved and restored to and from disk
- This was of course very slow (and increasingly so for larger programs which required a lot of state); it is one thing to save register context, and another to save the entire memory contents of a process!
- The clear solution is to hold multiple processes in memory and switch between them as needed, but this creates new responsibilities for the operating system in terms of virtualization, safety, etc.
	- The OS must provide a simplified **abstraction** of physical memory to user programs; this is the **virtual address space** (standard C view with code at bottom, then data, then heap and stack growing towards each other)
		- The size of a program's virtual address space is determined at various junctures, the last of which requires the OS:
			- First, the compiler will determine the minimum necessary size to simply store the code and static data (global variables, constants, etc.)
			- Then, the linker will create the final version of the code and thus determine the minimum size necessary to simply start up the program (depending on static vs dynamic linking), and include all relevant values in the headers of the executable file
			- Finally when the program is started, the kernel's **loader** will add space as configured for the dynamic portions of memory; stack and heap
			- Since memory is split into bytes, in $64$-bit systems memory addresses can theoretically go up to the $2^{64}$ byte which is $16$ exabytes (compared to just $4$ gigabytes for $32$-bit systems)
				- In practice, CPUs do not use all $64$ bits for addressing; there is no hardware configuration with that much RAM!
	- Processes must be properly **isolated**; no access to memory of other processes or the OS (although memory is shared between processes sometimes)
	- The OS itself must not utilize a large amount of physical memory or **overhead** time for its memory management mechanisms
# <u>15: Address Translation</u>
**Address Translation:** hardware-based address translation transforms the virtual memory address in each access into the corresponding physical address
- Before hardware support for address translation was available, a purely software method called **static relocation** was used; the **loader** would rewrite every address in an executable to the desired physical offset **on load**
	- This mechanism has obvious limitations for dynamic allocation (which happens at runtime, and thus cannot be translated on load)
- Early implementations of hardware-based methods started with two registers called **base and bounds**
	- Programs were written in assembly (or compiled) in a way that assumed that the program's address space started at $0$; on load, the OS decides where in physical memory that executable will go and sets the **base** register to that address
	- As the program runs, every memory reference is translated like $\text{phyiscal address}=\text{virtual address}+\text{base}$; because this happens at runtime and allows for changing the base even after a process has started running, it is also called **dynamic relocation**
	- The **bounds** (or size) register is set to the highest allowed virtual address; every virtual memory address generated by a process is compared against the bounds, and if it is greater the CPU will raise an exception which the OS will handle
	- The base and bounds registers (and more) are referred to as the **memory management unit** (MMU) of the CPU, and can only be changed in kernel mode via privileged instructions (by the kernel)

The base and bounds mechanism is clearly efficient (simple and requires just a couple of special registers in terms of hardware support), and does facilitate virtualization (easy memory abstraction from programs, no notion of physical memory) and protection (via bounds)
- However, so far we implicitly assumed that each address space is the same standard size, and so programs that use less memory than that standard size will cause **internal fragmentation** wherein the space between the heap and stack is unused but cannot be reallocated
- The base and bounds mechanism also has no way to support the possibility of an address space that is **larger than physical space**; the calculation of physical addresses from virtual ones would yield nonexistent physical addresses
- Processes that require more space than initially allowed by their virtual address space can also become tricky, as the entire process' memory would have to be moved if the physical addresses immediately following the process' highest order addresses are already allocated
- To begin with, the ideas of a standard size for each address space and virtual memory being mapped only to contiguous ranges of physical memory, are restrictive and wasteful; there is a better way (in Chapter 16)


We can now elucidate additional responsibilities for the OS:
- Firstly (chronologically), the OS must register handlers on boot for the various memory-related **exceptions** which may happen, such as memory out-of-bounds access, etc.
	- These handlers generally immediately terminate the offending process; this is different when the out-of-bounds happens in relation to the stack pointer or heap pointer (break) in which case the kernel will generally attempt to allocate more memory (different topic)
- When a **new process** is created the OS must find and allocate physical room for its virtual address space in memory
	- To do this the OS might keep a simple array (called a **free list**) of memory slots, but this is difficult to do when virtual address spaces are **not all the same size** or **necessarily smaller** than the physical memory
	- The OS must also manage memory deallocation from terminated processes on the free list (full treatment of free lists in Chapter 17)
- When a context-switch is performed, the OS must **save the base and bounds registers** on the process' PCB and restore them on resumption, since each process will be at different physical addresses and thus have a different base value
	- Note, this allows the OS to switch the physical location of a process entirely during a trap by copying the contents to a new physical location and updating the base and bounds values accordingly before restoring context
	- Setting the values of base and bounds registers will be **privileged** kernel-mode operations
# <u>16: Segmentation</u> 
**Segmentation:** each **logical segment** of a program's virtual address space (code, heap, stack) gets its own base and bounds; the segments can then be independently sized and put into **non-contiguous** parts of physical memory
- This means **less unused space is allocated** compared to simple base and bounds implementations; even if we have a **sparse** address space it is easier to start with **less internal fragmentation** and grow segments as needed
- Code-sharing for multiple instances of the same program or shared libraries is also easier to support with segmentation; the hardware must support an **additional register** to indicate whether a segment in physical memory is **read-only**
- The hardware must now support **three pairs of base and bounds** registers; one for each segment
- Note, the **virtual address space is still modelled as a contiguous**, but now when translating to physical addresses we need the **offset** from where the segment starts in virtual memory (since the segments in physical memory are now non-contiguous) rather than the full address
	- Ex. if heap starts at virtual address $4000$ and we get virtual heap address of $4200$ then $\text{offset}=200$, calculate the physical address as: $\text{heap base}+\text{offset}$
	- Ex. if $\text{offset}\geq \text{bounds}$, the hardware will raise an exception; the OS can handle this illegal address reference as the infamous **segmentation fault**, among other scenarios
- Recall the **stack grows downward**; the hardware thus needs an **additional register** bit flag to indicate growing downward
	- Physical addresses are then calculated as: $\text{stack base}+(\text{offset}-\text{bounds})$ 
	- Bounds checks are then done as: $|\text{offset}-\text{bounds}|\geq\text{bounds}$, is a segmentation fault

For any given virtual memory address, the OS must first **determine the source virtual segment** to do correct translation to a physical address:
- The **explicit** approach uses the highest order bit(s) of any virtual memory address to determine the segment, and the remaining bits as the offset; this approach has a couple of obvious limitations:
	- Segments are **limited in maximum size** by the number of remaining lower order bits used for offset, and thus as number of segments grows the size of the segments must decrease
	- The **possible number of segments** will always be a multiple of $2$; if there is an odd number of segments then the highest value of the segment bits simply marks invalid (effectively wasted) addresses, sometimes referred to as **virtual fragmentation**
		-  Ie. given an address space with $2$ segment bits: $00=\text{code}, 01=\text{heap}, 10=\text{unused}, 11=\text{stack}$
- The **implicit** approach has the OS determining the segment by context; ex. the program counter is always from the code segment


When context switching, the OS must now save all the various segment registers (bases and bounds, direction flags, read-only flags) and restore them upon restart of the process
- The OS must also **allocate physical memory** when a process requires more; this can be as simple as increasing the bounds value if more contiguous physical memory is available, or moving the entire location (base and bounds) of the segment if not
	- This raises the problem of **external fragmentation**; if there are many different-sized segments in physical memory, the sum of total unallocated memory might be large but spread among many small odd-sized fragments which cannot hold a segment
		- There are many algorithms to **compact** and re-fit segments to resolve fragmentation, but they tend to be incur a lot of OS overhead to relocate segments (among other drawbacks); there is yet again a better way! (in Chapter 18)
# <u>17: Free Space Management</u>
If we use mechanisms in which memory is divided into standard fixed-size units, then free space management is easy; simply keep a list of each slot and allocate them in order. However, we saw that segmentation allows for variably sized segments, which leads to external fragmentation.

We look at how the C standard library manages virtual memory (specifically the **heap**) as a **microcosm** of **free space management** and minimization of external fragmentation.

The data structure used to manage free space in the heap is generally called the **free list**; any number of data structures can be used to represent it, in our case we will use a linked list wherein each node contains the size of the chunk it represents
- When allocating, the two main mechanisms used are:
	- **Splitting:** on finding a free chunk large enough for the requested amount, the chunk is split such that its lowest addresses are allocated as needed and the list node is updated to the resultant smaller size
	- **Coalescing:** on free (or at other junctures), merge any free chunks that are adjacent into larger chunks
- Note that `free(void* ptr)` does not take any size parameter; it determines the size of allocated memory from a **header** which is a generated on `malloc(size_t size)` calls and sits immediately above the returned pointer
	- The header contains the size of the allocated memory and some other data for verification; `free()` then gets to the header by simple pointer arithmetic from the passed in pointer and then determines size as the field in the header plus the size of the header itself
		- It also follows that on `malloc(n)` calls, we search for free chunks of size $\text{n}+\text{header size}$
- The **free list itself is integrated into the heap**, when we initialize the heap with an `mmap()` call we use that as the pointer to the head and only node of our free list, with the size field being the size requested from `mmap()` minus the size of the node
	- On allocation, the head pointer is moved down as needed and the size updated, while the memory above gets a header for later use with `free()` and the pointer just below that header is returned to the requestor
	- On deallocation, the head pointer is updated to the header address of the now freed chunk, and the next field of this head is now the address of what was the head just before the deallocation
		- This is effectively a linked list insert at start; this will often break the **address-based ordering** of the nodes in the free list, but this can be minimized when doing coalescence on frees (checking the adjacent chunks rather than traversing linked list)

When we have an attempt to allocate but no sufficiently large free chunks exist, we could just fail to allocate, or we could ask the OS for additional memory:
- Note, none of what we discussed above were system calls, `malloc()` and `free()` are **C procedure calls**; but in the case of needing a larger heap for allocation, `malloc()` will make a system call (`sbrk` in the case of Linux, etc.)

On top of these mechanisms, we need a **policy for allocation**:
- **Best Fit:** perform an exhaustive search of the free list for the **smallest** free chunk that is sufficiently large for the request, but this leaves behind many very small free chunks
- **Worst Fit:** perform an exhaustive search of the free list for the **largest** free chunk that is sufficiently large for the request; this avoids the issue of small free chunks from best fit but still does not prevent external fragmentation in general
- **First Fit:** this is the policy we have assumed so far; simply traverse the free list and choose the first chunk that is large enough
	- This has the advantage of not needing to perform an exhaustive search of the free list, but tends to clog up the initial addresses with small allocations
- **Next Fit:** same as first fit, but keeps a pointer to the last chunk that was traversed so that the next allocation restarts traversal from there
- **Segregated Lists:** this manages a separate free list for very common allocation sizes; this resolves external fragmentation for those allocations and generally makes management a lot simpler
- **Buddy Allocation:** recall that coalescing can become complicated when the free list becomes unordered (relative to addresses represented by the nodes), buddy allocators like the binary buddy allocator try to resolve this:
	- The binary buddy allocator recursively divides free space by $2$ into pairs of buddies until chunks are as small as they can be to accommodate the request and then allocates the first free chunk
		- Then, when deallocating we simply recursively check if the buddy chunk at each level of division is also free when coalescing
	- This policy can suffer from some internal fragmentation as allocation requests will not always be perfectly divisible by $2$ 
# <u>18: Paging</u>
The base and bounds mechanism suffers from inflexibility and internal fragmentation. Segmentation suffers from external fragmentation due to variably sized segments. Paging mostly resolves both of these issues at the expense of more memory and time overhead.

**Paging:** virtual address space is divided into **fixed size**, contiguous, non-overlapping units called **pages**, and physical memory is viewed as an array of page **frames** of that same size
- The OS maintains a free list of all **unallocated** physical frames
- The OS maintains a **page table** per process, which stores the **address translation** mapping of $\text{virtual page}\to \text{physical frame}$

Translation of virtual addresses in a paging system fixes some of the deficiencies and limitations of segmentation:
- The size of the process' virtual address space and the page size determine the number of pages (by simple division)
- Based on the page size and number of pages, we can determine how many of the highest order bits in a virtual address map to the page number and how many to the offset
	- All of this is essentially modulo division so the **entire virtual address space is covered by definition**, compared to segmentation where segments varied in size and thus could lead to constraints with unaccessible virtual addresses
	- The higher order bits make up the **virtual page number** (VPN) and this is the only part of the address that is translated into a **physical frame number** (PFN); the **offset is the same** in the virtual and physical addresses
		- The offset was technically the same in virtual and physical addresses in base and bounds and segmentation as well; however it was used to calculate an entirely new address, whereas with paging we simplify this by using binary modulo arithmetic
- Recall from Chapter 13 that modern systems use $\geq 32$ bits to address memory, which leads to page tables with millions (or even more) of entries depending on the page size (which determines the number of pages / bits used for the VPN)
	- Ex. for a $20$-bit VPN, we would have $2^{20}$ (roughly a million) entries in the page table, and if each entry is a $32$-bit ($4$-byte) unsigned integer, then the page table for a single process would take up $4$MB in memory
		- When running hundreds of processes simultaneously, the OS memory overhead of all their page tables becomes significant!

The page table holds the map of $\text{VPN}\to \text{PFN}$ that is used for translation
- The simplest implementation of a page table is the **linear page table** which is an array **indexed by VPNs**, so $O(1)$ lookup!
	- Each **page table entry** (PTE) is usually an **unsigned** numerical type interpreted as a sequence of **bit flags**, ex. in x86 architecture these sequences of bits hold:
		- The PFN with the twelve highest order bits
		- A **valid bit** is used to indicate whether a translation is valid; ie. the initial empty space between the stack and heap is **invalid** and thus **does not get allocated any physical frames** which allows us to save memory with **sparse** address spaces (more on this in Chapter 20)
		- Some **protection bits** used to indicate whether a page can be read from, written to, or executed from, allowing the CPU to raise an exception if a frame is accessed inappropriately
		- A **present bit** to indicate whether a page is in a physical frame or on disk (more on this in the chapter on swapping)
		- A **dirty bit** to indicate whether a page has been modified since being loaded into physical memory
		- A **reference / accessed bit** to track whether a page has been accessed
- When allocating memory during process initialization, the OS creates the page table and uses it for translations thereafter; ex. if the process attempts to access an address which translates to an invalid VPN, or a VPN with a PTE with valid bit low, the OS must handle it
- The **page table's physical address** in **kernel memory** is held in a **page table base register**
	- For a given PTE we determine its address, then extract the PFN, then concatenate it with the offset
	- To perform any memory reference from a user process, we must first do a memory reference within the kernel memory to find the PTE; this bit of code is missing several flag checks, and consider it must be done on each and every single memory reference!
```
VPN = (virtual_address & VPN_mask) >> shift_amount        // VPN_mask and shift_amount determined by
PTE_addr = page_table_base_register + (VPN * sizeof(PTE)) // bit length of VPN and offset
PFN = *(PTE_addr) & PFN_mask >> shift_amount              // PFN_mask and shift_amount determined by
offset = (virtual_address & offset_mask)                  // location of bit flags in PTE
phys_addr = (PFN << shift_amount) | offset
```

To sum up, the benefits of paging are:
- Simplifies memory management (ex. stack is treated no differently; part the stack and part of some other segment can be in one page)
- Fixes external fragmentation (divides the virtual address space into fixed size pieces) and thus simplifies free space management
- Minimizes internal fragmentation

However, using paging as the core mechanism to support virtual memory can lead to high performance overheads, because the address mapping information is stored in physical memory. Thus, paging requires an extra memory lookup for each virtual address generated by the program. Going to memory for translation information before every single instruction fetch or explicit load or store is prohibitively slow.

We will need help from the hardware MMU to help mitigate these memory and computation overhead downsides.
# <u>19: Translation Lookaside Buffers</u>
**Translation Lookaside Buffer:** a part of the CPU's MMU, a TLB is a hardware cache that stores popular virtual to physical address translations
- On each virtual memory reference, the CPU first checks the TLB for the desired entry
	- If there is a TLB hit, the translation is done without having to perform the additional memory access to search the page table
	- If there is a TLB miss, the hardware must access the page table in memory and update the TLB with the relevant translation, then retry the instruction

The TLB **hit rate** is defined as $\frac{\text{TLB hits}}{\text{total number of mem accesses}}$
- **Spatial locality** of memory accesses allows us to have a higher TLB hit rate, as the initial miss will update the TLB and thereafter all accesses from the same page will be hits
- Hit rate is affected by **page size**; larger pages take advantage of the benefits of spatial locality to increase hit rate and reduce page table memory overhead, however they also increase internal fragmentation and page-in latency (more on the latter part in Chapter 21)
- The size of the TLB itself is of course also limited, so **temporal locality** of memory accesses will also increase hit rate as it decreases the probability that the translation had to be cleared from the TLB
- TLB **coverage** is a related issue; if a program accesses too many virtual addresses in quick succession to fit into the TLB, then there will be many misses (this can be alleviated with larger page sizes but again with the aforementioned drawbacks) 

There are two commonly found mechanisms for handling TLB misses:
- **Hardware-managed TLBs** are what we initially assumed and are common on CISC CPUs; on a TLB miss the hardware uses the page table register to access the relevant memory and walk through the page table to find the translation and update the TLB, then retries the op
	- This mechanism has the disadvantage of being inflexible in the format of the page table; the OS must strictly adhere to the necessary format when initializing the table for the hardware to be able to parse it
- **Software-managed TLBs** are common on RISC CPUs; on a TLB miss they raise an exception, pause the current instruction and go to the OS trap handler which must then update the TLB with a privileged operation before returning, after which the CPU retries the instruction
	- With this mechanism, the return from trap is unique in that it must **return to the same instruction** (a retry after TLB miss) that caused the trap, rather than the next instruction as in all other cases; this is handled by the CPU when it saves the program counter
	- This mechanism must also be careful not to create an **infinite chain of misses**; either by keeping OS trap handler code at **unmapped** physical locations not subject to translation or by creating permanent **wired** translations in the TLB
	- The major advantage of this mechanism is total flexibility for the OS in the data structures it chooses to implement the page table

The contents of TLB entries are similar to a page table but not exactly the same:
- Each entry will have a VPN and PFN for translation, as well as other bit flags:
	- The **valid bit** indicates whether an entry holds a valid translation (note this is slightly different than the meaning of the valid bit in page table entries)
	- Some **protection bits** which indicate how a page can be accessed, same as in page tables
	- An address space identifier field, as defined below

The TLB is only for the **process currently in context** (VPN indexing would be impossible otherwise). This is important for context-switching:
- One option is to simply **flush** the entire TLB on context-switches
	- Hardware-managed TLBs can do this directly on any change of the page table register
	- Software-managed TLBs can do this through a privileged instruction while in kernel mode in a context-switch
	- The major disadvantage of resetting the TLB like this is that the **hit rate restarts** at $0\%$ again when this process is re-queued; in case of an OS that is switching between several processes very often for whatever reason, the hit rate will remain low
- Some hardware supports an **address space identifier** field in TLB entries; the ASID is essentially a shorter PID which then allows context-switches to happen without the need for flushing the TLB since entries are now identified with a combination of VPN and ASID
	- This also requires an ASID register in the hardware to compare against

Lastly, we have to consider how to handle **cache replacement**; when the TLB is full which entries should be replaced to maximize hit rates. This problem requires looking into swapping (Chapter 21), but we highlight a few typical policies here:
- The **least recently used** policy simply replaces the LRU entry; this policy deduces that such an entry has low spatial and temporal locality to whatever is currently being run
	- There are edge cases where LRU performs poorly; ex. code that loops over memory addresses that are each split over $n+1$ pages with a TLB of size $n$ (in this case the hit rate is $0\%$)
	- For simplicity and avoidance of edge cases like the above, a common policy is to replace **random** entries
# <u>20: Smaller Tables</u>
TLBs resolve the additional memory access overhead of paging, but we still have the issue of page table size; recall, for a $32$-bit address space with $4$KB ($2^{12}$) page size, we have roughly a million ($2^{20}$) pages each holding a $4$B integer. For hundreds of processes this adds up fast!
- A simple solution could be **larger page size**, we already mentioned some benefits of this for TLB hit rate; this also **reduces the number of PTEs**, but at the expense of **increased internal fragmentation**
- A **hybrid** solution with segmentation and a page table for each segment is also possible; this reduces the need for the pages between stack and heap to be held in the page table just to be marked invalid, but it **reintroduces external fragmentation**

An approach that tackles the same issue as the segmentation paging hybrid, but does not reintroduce external fragmentation, is the **multi-level page table** (this is the approach used by x86, which recall uses hardware managed TLBs)
- In a multi-level page table, the page table register is replaced with a **page directory register** (which holds its physical address)
- The **page directory** is a linear table, the entries of which hold the **PFNs to page tables**, and a **valid** bit flag which is high when there is **one or more valid PTEs** in that frame and low otherwise
	- The page directory itself is meant to occupy a **single frame** in physical memory, and each entry in the directory is the same size as a PTE (recall, that size is $32$ bits, ie. $4$ bytes)
	- Frames that would hold only invalid PTEs have empty addresses in the page directory, and thus those frames do not need to actually be allocated for the page table; in this way the **page table is now non-contiguous**
		- This resolves the problem of the **pages between stack and heap being held in page table entries just to be marked invalid**
		- Now we merely need to allocate a single PDE to determine if that entire frame would have had valid translations or not, rather than an entire frame of invalid PTEs
		- This approach is essentially paging for the page table; now if we need more memory we simply find an unallocated frame and add the relevant PTEs there, rather than changing valid bits in a linear page table in contiguous memory
		- The one disadvantage is that on TLB misses we now have to do **one more memory access** (to the page directory and then to the page table) than we did with linear page tables (only to the page table)

Consider how translation (or more accurately, TLB updating) works with a multi-level page table:
- Any given virtual address will have its the number of offset bits determined by the page size, leaving us with the bits that make the VPN
- To index into the page directory, consider that the **number of PDEs is the number of pages used to hold the entire page table** (alternatively, the size of a frame divided by the size of a PDE); this number determines how many of the high order bits of the VPN we use to determine the PDIndex
	- Using this PDIndex we index into a PDE, get the PFN of the frame of the page, then use that PFN and the remaining bits of the VPN to index into the page table (this amount of bits is also determined by the number of PTEs that fit in a page); the pseudocode below outlines this
```
PDIndex = (VPN & PD_MASK) >> PD_SHIFT     // Get the PDIndex
PDEAddr = page_directory_base_register + (PDIndex * sizeof(PDE))  
PDE = AccessMemory(PDEAddr)               // Get the PDE
if (PDE.Valid == False) RaiseException(SEGMENTATION_FAULT) else
PTIndex = (VPN & PT_MASK) >> PT_SHIFT     // Get the PTIndex
PTEAddr = (PDE.PFN << SHIFT) + (PTIndex * sizeof(PTE))
PTE = AccessMemory(PTEAddr)
```

When page sizes get very small, the **size of the page directory increases** and ultimately negates the memory overhead benefits of the multi-level page table approach; in these cases we introduce several layers of page directories and split the VPN into further hierarchies.

There are other unique solutions like **inverted page tables**; the system has a **single page table** that has an entry for each physical frame of the system, the entries are queried via a hash function and tell us which process (and which page in that process) is using this frame


A **final note on address translation**:
- Given some common sizes of an address space in bytes, to convert to the bit representation of an address (virtual or physical) and back:
	- $2^{10}=1024$ bytes ($1$ KB), and $10$ bit addresses
	- $2^{12}=4096$ bytes ($4$ KB), and $12$ bit addresses
	- $2^{14}=16384$ bytes ($16$ KB), and $14$ bit addresses
	- $2^{15}=32768$ bytes ($32$ KB), and $15$ bit addresses
	- $2^{20}=1048576$ bytes ($1$ MB), and $20$ bit addresses
- If given a virtual address, then will also be given the size of the VPN or offset (ie. page size); use same calculation as above to determine how many bits represent the VPN and the offset (recall VPN is highest order bits), from there can translate addresses to PTEs and extract relevant PFN bits and append offset contents
- Then various metrics:
	- **Total number of virtual pages** (ie. PTEs) is the $2^{\text{number of VPN bits}}$
	- **Size of the page table** is then $2^{\text{number of VPN bits}} \times 4$ bytes since each PTE is $32$ bits
- Hexadecimal representations for binary are commonly used; recall, each hexit represents $4$ bits
# <u>21: Swapping Mechanisms</u>
So far we have assumed that the address spaces of all concurrently running processes together fit into physical memory. Now we relax this assumption to see how we can support concurrent large address spaces by using the (slower and larger) **hard disk drive**.

Consider a TLB miss; the hardware (or the OS if software-managed) then checks for the relevant VPN in the page table but finds that the **present bit** in the PTE is low, meaning the page is not in physical memory but on disk, and thus a **page fault** exception is raised
- The OS is invoked to handle this page fault through its **page fault handler**, which will have to know where on disk the page is
- The OS **partitions** some space on disk as a **swap space**, and now to swap pages in and out of this space, the OS will need to know the **disk address** of a given page; this address is usually stored in the PTE in bits that have other uses when the page is in memory
- When doing **swap-ins** and **swap-outs**, we need a carefully crafted policy when choosing pages (see next chapter); the wrong policy will drastically decrease performance to disk-like speeds!
The OS does not only do swap-ins and outs when memory is entirely full; there is generally a **low watermark** and **high watermark**, and a background **swap daemon** that swaps pages in and out as necessary to keep the number of free frames in memory between these watermarks
- This allows swaps to happen in batches, called **clustering**, which helps amortize the relatively long disk read and write times (disk drives perform a single large write more efficiently than many small ones)
# <u>22: Swapping Policies</u>
A **page replacement policy** is how the OS decides which pages to evict to disk, and plays a crucial role in overall system performance. We want to optimize **average memory access time**, where:
$$
\text{AMAT}=\text{T}_{\text{mem}}+\text{T}_{\text{disk}}\cdot P(\text{miss})
$$
Since we know strictly $\text{T}_{\text{mem}}<\text{T}_{\text{disk}}$, the **optimal** replacement policy is one that minimizes $P(\text{miss})$; it is proven that such a policy is one that replaces the page that will accessed **furthest in the future**
- Unfortunately, implementing this optimal policy is quite difficult, or worse yet impossible as we do not know the future; however, it serves as a comparison point to judge other replacement policies by (when using fully determined test scenarios)
- Actual replacement policy implementations include:
	- **FIFO**; simple, but has no concept of the possible importance of any given page (note **queue is not updated on hits**)
	- **Random** (uniform); same benefits and drawbacks as above
	- **LRU**; utilizes concept of temporal locality to determine page importance, but more overhead to implement (and **queue is updated on hits**)
	- **LFU** (least frequently used)
Consider how various memory reference traces affect our choice of replacement policy:
- For a trace with no or **low locality**, all the replacement policies **perform roughly the same** and improve linearly with increasing cache size; while the optimal policy is convex downward (when cache size reaches the size of the entire trace, hit rate is $100\%$ for all policies)
- For traces where the vast **majority** of references are to a small **minority** of addresses, LRU logically does better than random or FIFO
- For traces that are **looping and sequential**, recall that LRU (which with such traces is logically equivalent to FIFO) has a shocking $0\%$ hit rate until the cache size reaches the size of the trace at which point it becomes $100\%$

Implementation of these policies can become computationally expensive; for LRU, we would have to hold and update some kind of flag for each page every time it is accessed, and then on swaps scan possibly millions of pages to determine the least recently accessed one!
- By comparison FIFO only has overhead on swaps, not on each memory access
- We can **approximate** LRU; recall the **reference bit** that is held in PTEs, the hardware **sets this bit to high whenever the page is accessed**, then at swap time an OS managed **clock algorithm** goes thru pages and sets this bit low until it finds a page where it is already low to swap
	- The kernel checks if the pointed to page's reference but is high; if it is then it **sets it to low but increments the clock hand** to the next page, until it finds a page where the bit is low and swaps that page (with bit high since it was just referenced to require caching to being with, and increments the hand)
	- The worst case performance for this algorithm is if all pages have this bit high; the clock starts at some arbitrary page but then has to walk through all the pages and set each bit to low until it finally lands on the one it started on (which on this second go already has the bit set to low)
	- A slight improvement to this uses the **dirty bit** of a PTE; if the contents of a page have been changed, then when swapping out it must be written back to disk, whereas if it hasn't then we have that data already in its original place on disk and only need to swap in 

Beyond page replacement there is also **page selection policy**; when should the OS swap-in a page from disk to memory:
- Most common is **demand paging**, which means the page is swapped-in when there is an attempt to access it and hardware invokes the page fault handler
- **Prefetching** is used when the OS can reasonably assume that a page will soon be accessed; most often this happens with code which is split across multiple pages

Lastly, if there are too many processes and **memory pressure** gets too high, we encounter a condition called **thrashing**; when the OS spends more of the CPU resources on swapping than on executing processes
- In such cases the OS can perform **admission control** where it essentially modifies its scheduling behaviour to prioritize a small subset of processes whose memory demands can be satisfied by physical memory and that can thus run to completion without much swapping
	- More crude approaches have the OS simply terminate memory-intensive processes!
# <u>23: The Linux Virtual Memory Systems</u>
In real virtual address spaces, the code segment never begins at page $0$; this page is marked inaccessible, in order to provide support for detecting **null pointer accesses**.

Linux on x86 is most common, and recall x86 does hardware-managed TLBs; so Linux sets up page directories and page tables as dictated by the architecture, and sets the page directory pointer in a privileged operation
- In x86-64 **four-level** page tables are used; recall only the lowermost $48$ bits of a memory address are actually used, so with the standard $4$KB page size (meaning $12$ bits for offset) we are left with $36$ bits for the VPN, which is further decomposed by $9$ bit pieces for directories
- There is support for larger pages (ex. $1$GB); recall, this is only strictly necessary for database applications but also helps increase TLB hit rate in general; this used to be done explicitly through process `mmap()` or `shget()` calls but now Linux does it opaquely as well

Furthermore, the **kernel address space** is part of each user process' address space (in the higher order addresses), however the page(s) holding this segment is **mapped to the same physical address** for all user processes
- This allows the kernel to virtualize its memory usage as well, which provides the advantages of no external fragmentation, swapping, and of course being able to write the kernel as a regular C program with pointers being virtual addresses!
- This also makes it easy for the kernel to work with pointers passed from user applications, since the page directory, page table, TLB etc. can all be used for address translations during syscalls
- This is where the **protection bits** from PTEs come in; these determine what mode the CPU needs to be in to access a page (ie. kernel mode to access the kernel virtual address space)
- In $32$-bit Linux, one quarter of the virtual address space (starting at $0\text{xC}000000$) is reserved for the kernel, and is divided into two types:
	- **Kernel logical addresses** are allocated within the kernel by calls to `kmalloc()`; most kernel data structures live here (ie. page tables, per-process kernel stacks, etc.) and thus this memory is **never swapped out to disk**
		- This memory is **always directly mapped** to physical memory starting at $0\text{x}0000000$; this makes translation very easy (no need for the lengthy translation sequences used for virtual addresses in general) and thus are **contiguous** in physical memory
	- **Kernel virtual addresses** are allocated within the kernel by calls to `vmalloc()` which returns a pointer to **virtually contiguous** region of memory of the requested size; this is virtual memory in the same sense as it is for user processes

In every PCB in Linux, there will be a struct called `mm_struct` inside of which there are `vm_area_struct`:
- Recall from Chapter 17 that allocating and growing various segments is done by allocators, not by anything directly related to the page table
- A **virtual memory area** divides the virtual address space into contiguous regions with different modes (read, write, execute, shared, locked) ex. a shared library in the code region would then be set to **shared**
- Linux does not allocate these areas on a `malloc()` call, and only maps them to physical memory when the memory is actually going to be needed 
- The `mm_struct` keeps an **address-ordered** linked list of `vm_area_struct`, each of which holds pointers to its beginning and end

**Sharing memory** is an important feature in Linux for reducing memory impact:
- Calling `mmap()` on an opened file descriptor returns a pointer to the beginning of a region of virtual memory where the contents of the file seem to be; then using that pointer, a process can access any part of the file with a simple pointer dereference
- There are many use cases where **processes want to communicate to each other** through memory; paging can accommodate this:
	- Page tables for two different processes can have PTEs which contain the same PFN; then each process can set different **protection bits** ex. to define a relationship where one process can read (but nothing else) a certain region of memory that is being written to by another process
	- A common use case for this is `fork()` and `exec()`, and indeed if we could not share this way then we would have to duplicate an entire copy of the  parent's memory contents first before inevitably overwriting them
		- To avoid this Linux does **copy on write**; **CoW** creates shared mappings of a parent's pages, sets them to read-only and has the child reference those; a new private copy of a page is created for the child only if and when it tries to write to that page

Recall page replacement policy; Linux has an additional consideration:
- **Local replacement** is when a page fault leading to additional memory allocation can only replace a frame that is from its **own set**
- **Global replacement** allows a page fault to replace a frame from any process; Linux implements this using a **2Q** replacement policy, with two page lists:
	- An **active list** (Am) of frequently used pages, managed using something akin to the clock algorithm LRU approximation
	- An **inactive list** (A1) of less frequently used pages, managed roughly in FIFO order
	- Pages are initially put on the inactive list, and have a bit flag set high on first access; followed by being moved to the head of the active list on another access (with bit flag already high)
	- When memory pressure is high, pages are demoted from the tail of the active list to the head of the inactive list, and pages are swapped out entirely from the tail of the inactive list
# <u>26: Concurrency</u>
A **multi-threaded** program has more than one point of execution; ie. **multiple program counters**, each of which is being fetched and executed from. In this sense **threads** are like processes except for one major difference; they **share the same address space** and thus access the same data.
- Like processes, threads have their own register context, and thus **context switches are necessary** when interleaving thread execution
	- With processes, register context was saved to PCBs; with threads register context is stored to **TCBs**
	- However, since threads share an address space, context switches between threads do not require changing of the page table (or page directory) base register, nor any flushing of the TLB
- While threads share the same address space, they do each get their own **thread-local storage** in the form of a **stack for each thread** (this creates more complexity around memory allocation and how each stack grows, but stacks are usually relatively small unless a program uses deep recursion)
	- Recall, aside from the stack all other memory (code, global data, heap) is shared
The benefits of multithreading are mostly realized in multi-processor architectures, however even with a single CPU the kernel overhead of creating threads and switching between them is less than that of creating and switching between processes (and certainly less than for piping data between processes etc.)

The major problem that sharing an address space creates is **data races**, which can lead to a program being unexpectedly **indeterminate**
- Chunks of code that must not be executed concurrently by more than one thread against a **shared resource** (some variables or data structures) to prevent indeterminate results are called **critical sections**
- Data races are a type of **race condition**, and arise if multiple threads enter the critical section at roughly the same time and attempt to update a shared data structure, leading to surprising and perhaps undesirable outcomes
- Enforcement of **mutual exclusion** guarantees that only a single thread enters a critical section at a time, thus avoiding races and resulting in **deterministic** program outputs
- We cannot know ahead of time how the kernel scheduler will schedule thread execution, and since even simple C code often maps to multiple processor instructions (ie. not **atomic**), preemptions can lead to unexpected effects between loads from memory and writes back to memory

To solve the problem of **non-atomicity** leaving **program output to the whims of the scheduler**, we again need hardware support; a few useful instructions that allow us to build **synchronization primitives**
- **Synchronization** in this context refers to coordination of thread execution relative to other threads; each thread has its program order (as defined by the code) but execution of the broader program is some scheduler-determined interleaving of those per-thread orders
- The hardware will provide atomic operations that execute as a single **indivisible** logical unit despite spanning multiple instructions
# <u>27: Thread API</u>
The Linux thread API is defined in `pthread.h`, where threads are represented by `pthread_t`:
- To **create** a thread (the execution of which is defined by the code of some function):
	- `int pthread_create(pthread_t* thread, pthread_attr_t* attr, void* (* start_routine)(void*), void* arg)`
	- Note, the types in the `start_routine` function pointer (which is effectively the thread's code) can be changed to data types if necessary for the given use case; for pointers keeping pointer as `void*` allows for the most flexibility (simply cast as needed)
	- The type of `arg` of course must match what is expected in the function pointer
- To **wait** for a thread to complete:
	- `int pthread_join(pthread_t thread, void** ret_value_ptr)`
	- Note, not all multi-threaded programs will follow this sequence of creating and waiting; ex. a web server may create threads and leave them running for the entire uptime of the program
- To use **locks** (represented by the **mutex** type; short for mutual exclusion):
	- `pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER` or `int rc = pthread_mutex_init(&lock, NULL)`
	- `int rc = pthread_mutex_lock(&lock)`
	- `int rc = pthread_mutex_unlock(&lock)`
	- `pthread_mutex_destroy(&lock)` if there is a point in the main thread where it will continue to run but the lock will no longer be used
- To use **condition variables** (represented by `pthread_cond_t`):
	- `pthread_cond_t cond = PTHREAD_COND_INITIALIZER`
	- `int pthread_cond_wait(pthread_cond_t* cond, pthread_mutex_t* mutex)
	- `int pthread_cond_signal(pthread_cond_t* cond)`
# <u>28: Locks</u>
To ensure that critical sections of programs run atomically, we use **locks**
- Locks are **global or main thread local variables** in a program which **hold a state** (of the lock, and possibly other data such as which thread holds the lock or an intrusive queue); the lock is either **locked** meaning a single thread holds the lock, or **unlocked** meaning no threads hold the lock
- The `lock(lock_t)` and `unlock(lock_t)` procedure are used to interact with the lock; a call to `lock()` while the passed in lock is locked will **block the calling thread** until the lock is acquired, thus preventing it from accessing the following critical section (which these calls are wrapped around)

To create lock functionality, the hardware and the kernel will have responsibilities. We have several metrics by which to evaluate our lock implementation:
1. The lock must facilitate **mutual exclusion**
2. Does each thread contending for the lock get a fair shot at acquiring it; ie. is the implementation **fair**, does it prevent starvation
3. What is the overhead and impact on **performance** of various scenarios, ie. simple lock and unlock, having a thread blocked while waiting for a lock, etc.

One of the earliest and simplest solutions was to **disable interrupts**; the `lock()` call would make a syscall to disable interrupts and the `unlock()` call to reenable interrupts
- The major benefit is the simplicity, but the drawbacks are essentially everything else; today the instruction to disable and enable interrupts will be privileged in most hardware, and modern kernels are unlikely to offer syscalls to user programs to request these instructions
	- Recall, this restrictiveness is to prevent a malicious or faulty program from fully monopolizing control of the system away from the kernel
	- This approach is also not feasible as long critical sections can lead to missed interrupts (ie. I/O completion) leading to erroneous system behaviour, unresponsiveness, mismanagement of multiple CPU architectures, etc.
The next simplest solution would be to simply have our lock data structure have a **flag** which indicates whether it is held or free; then `lock()` checks and sets the flag, `unlock()` clears it, and `lock()` also **spins** in an empty loop if the lock is already held until it is free
- The first problem with this solution is that it is **incorrect**; if the `lock()` call is invoked on a free lock but then preempted by another thread before it can set the flag to high, then we can have multiple threads that own the lock; we have failed to assure mutual exclusion
- The second problem is that there is no concern for **fairness**; the thread's ability to acquire the lock is entirely up to the whims of the scheduler
- The third problem with this solution is the **performance impact of spinning**; especially on a single processor system, if a thread spins while waiting for a freed lock, those CPU cycles until the scheduler switches it out are completely wasted (ie. the thread that actually holds the lock cannot run!)
```
typedef struct __lock_t { int flag; } lock_t; 

void init(lock_t *lock) { lock->flag = 0; } 

void lock(lock_t *lock) { 
	while (&lock->flag == 1) 
		; // spin-wait (do nothing)
	&lock->flag = 1;
} 

void unlock(lock_t *lock) { lock->flag = 0; }
```

To resolve the issues with disabling interrupts or non-atomic load-and-store sequences like with flags, we need **atomic operations from the hardware** (or more complicated and expensive algorithms...so we us hardware assistance instead)
- The earliest and simplest operation that CPUs provided for these purposes is called a **test-and-set** or **atomic exchange**; this instruction stores a new value into a memory location and returns the previous value
	- `int TestAndSet(int* old_ptr, int new_value)`
	- This instruction now allows us to **correctly** implement `lock()` as a **spin lock**; having this function use the atomic instruction resolves the issues regarding mutual exclusion that were observed in the simple flag solution from before
	- Unfortunately, this instruction does not facilitate any improvement in **fairness**, and **performance** may still be poor on a single CPU (ie. run $n-1$ of $n$ threads, each of which spins for its time slice before circling back to holding thread)
```
void lock(lock_t *lock) { 
	while (TestAndSet(&lock->flag, 1) == 1) 
		; // spin-wait (do nothing) 
} 
```
- The **compare-and-swap** operation tests whether the value at the a specified address is equal to an expected value and if so updates that memory location pointed with the new value, and returns the original value at that memory location
	- `int CompareAndSwap(int* ptr, int expected, int new)`
	- When used in a spin lock implementation this instruction is effectively the same as test-and-set
- Lastly the **fetch-and-add** instruction atomically increments a value at a memory address while returning the old value
	- `int FetchAndAdd(int* ptr)`
	- This instruction is used to implement a **ticket lock** (a form of spin lock); we have a lock struct which has ticket number and turn fields, and we spin until the lock turn is our turn
		- Each `lock()` call uses fetch-and-add for the ticket field of the lock and spins on checking turn, while `unlock()` simply increments turn
		- This solution gives us mutual exclusion on entry of the critical section, and now unlike test-and-set or compare-and-swap we get FIFO **fairness** albeit with no change to **performance** (ie. this is still a type of spin lock)
```
typedef struct __lock_t { int ticket; int turn; } lock_t; 

void lock_init(lock_t *lock) { lock->ticket = 0; lock->turn = 0; } 

void lock(lock_t *lock) { 
	int myturn = FetchAndAdd(&lock->ticket); 
	while (lock->turn != myturn) 
		; // spin 
} 

void unlock(lock_t *lock) { lock->turn = lock->turn + 1; }
```
- On the MIPS architecture the **load-linked** and **store-conditional** instructions can be used in tandem to build locks and other concurrent structures

None of the **spin lock solutions** above have an answer for the **performance** issue of what to do when a context switch occurs in a critical section, and threads start to spin endlessly, waiting for the interrupted (lock-holding) thread to be run again. Consider some approaches:
- The simplest solution would be to **yield** the CPU whenever a thread attempts to take a held lock rather than spinning, but this does not alleviate the overhead costs of context-switching (which can add up as number of threads increases), nor does it address fairness
The solution is **sleep locks**, but these require OS support for putting threads to **sleep** and waking them, on top of hardware atomic instructions outlined above

Solaris provides the `park()` and `unpark()` syscalls for threads:
```
typedef struct __lock_t { 
	int flag; 
	int guard; 
	queue_t *q; 
} lock_t; 

void lock_init(lock_t *lock) { 
	lock->flag = 0; 
	lock->guard = 0; 
	queue_init(lock->q); 
} 

void lock(lock_t *lock) { 
	while (TestAndSet(&lock->guard, 1) == 1) 
		; //acquire guard lock by spinning 
	
	if (lock->flag == 0) { 
		lock->flag = 1; // lock is acquired lock->guard = 0; 
	} else { 
		queue_add(lock->q, gettid()); 
		lock->guard = 0;
		park(); // send thread to sleep 
	} 
} 
	
void unlock(lock_t *lock) { 
	while (TestAndSet(&lock->guard, 1) == 1) 
		; //acquire guard lock by spinning 
	if (queue_empty(lock->q)) lock->flag = 0; // let go of lock; no one wants it 
	else unpark(queue_remove(lock->q)); // wake one sleeping thread, pass the lock  
	lock->guard = 0; 
}
```

Linux provides the **futex** type (fast userspace mutex), which uses bitwise operations on a value in a defined address in memory to determine sleeping, waking, and queueing behaviour.
# <u>30: Condition Variables</u>
Locks are not the only primitives that we need to build concurrent programs; in particular, there are many cases where a thread will like to check a **condition** which may be altered by other threads before continuing execution; the most common example is a parent thread waiting for a child thread to complete
- The simplest solution would be a **global variable** which is mutated by the child and checked by the parent on a **spin**, but we have already covered the performance drawbacks of spinning before; we would prefer to put the waiting thread to sleep until the condition is met

**Condition Variable:** an explicit queue that threads put themselves on when some condition is not as satisfied (this indicates they are waiting on the condition); another thread, when it changes said condition, can then wake one (or more) of those waiting threads and thus allow them to continue
- Note the condition itself will still generally be represented by a **global variable** (or main-local); the condition variable is specifically the queue implementation provided by the kernel that facilitates the functionality above, it is (confusingly) **not** the variable that determines the condition
- A condition variable has **two procedure calls** associated with it:
	- `pthread_cond_wait(pthread_cond_t* c, pthread_mutex_t* m)` is executed when a thread wishes to put itself to sleep; `wait()` then:
		1. **Frees the passed in (assumed to be locked) lock**
		2. **Puts the calling thread to sleep** (atomically) and change its status from **running to blocked**
		3. **Enqueues the calling thread in the condition variable's queue**
		- The thread that awakes when signaled must of course re-acquire the lock before resuming execution (ie. re-checking the while condition on the global variable); luckily the `wait()` call has **built-in functionality to re-acquire the lock before it returns** to the calling thread
	- `pthread_cond_signal(pthread_cond_t* c)` is executed when a thread has changed the relevant state in the program and thus wants to wake **one** thread sleeping on this condition (there is **no guarantee of which thread** will be woken!)
		- Note, this call **does not block or force a context switch** of the calling thread, execution continues until the kernel scheduler intervenes as usual; thus, this call is often immediately followed by an `unlock()` call so that the waiting thread(s) can get a chance at the lock
	- Since both of these calls read and / or write to a global variable, they are **critical paths** (global variables are shared state) and thus must be wrapped in a lock and unlock sequence
		- Otherwise, consider a race condition wherein the parent checks the global variable and is then preempted **before calling** `wait()`; the child then runs, updates the variable and calls `signal()` to no one, after which the parent calls `wait()` for a signal that will never come because it already passed!

The **producer-consumer problem**, also known as the **bounded buffer** problem, is the most common real world (non trivial) example of condition variables:
- When we have multiple consumers, we have to consider what a signal to a sleeping consumer really means
	- With **Mesa semantics** signalling a thread only wakes them up; it is thus only a hint that the relevant state has changed (ie. that a value has been placed in the buffer), but there is **no guarantee** that when the woken thread runs the state will still be as desired (ex. another consumer grabbed it!)
		- Thus, threads should **always use while loops** rather than if statements when checking the relevant global buffer (or flag, etc.) for this reason; if the state changed yet again since the signal was sent but before the waking thread ran, the while condition re-check would catch this!
	- **Hoare semantics** is harder to build but provides a stronger guarantee that the woken thread will run immediately upon being woken, but is rarely used in real world code
- It is imperative that we use **separate condition variables** for conditions that producers and consumers may wait for, even though they are interacting with the same buffer; ie. the producer(s) may wait on the **empty** condition and signal the **filled** condition, and vice versa for the consumer(s)
	- With only one condition variable a consumer may signal that a buffer is empty after consuming, only to awaken another consumer which was waiting on that one same condition variable; that consumer would promptly go back to sleep and the parent would never wake up to begin with!
- Instead of all the above, issues related to multiple producers or consumers can be resolved by using a call like `pthread_cond_broadcast(pthread_cond_t* c)` (which wakes **all** threads sleeping on this condition) rather than `signal()` (wakes one)
	- This is called a **covering condition**; but this is of course less performant
# <u>31: Semaphores</u>
**Semaphore:** an object with an integer value, represented in glibc in `semaphore.h` by `sem_t` ; the initial value is key in determining the semaphore's behaviour
- `void sem_init(&s, 0, 1)`; the second argument will usually be set to $0$ to indicate that this semaphore is shared between threads of the same process, and the third argument indicates the **initial value** of the semaphore
- `int sem_wait(sem_t* s)`; **decrements** the integer of the semaphore by one, and puts the thread to **sleep** and blocks only if the resulting value is **negative** (otherwise simply returns immediately)
- `int sem_post(sem_t* s)`; **increments** the integer of the semaphore by one, and **wakes a waiting thread** (no guarantee of which one) if there are threads waiting on this semaphore
- Note, the internal implementations of all of these calls use the aforementioned syscalls to ensure **atomicity**
- Consider how the `wait()` call embeds the basic condition variable waiting logic, but note that this does not satisfy requirements of more complex conditions 

Semaphores can be used as locks, conditional variables, or both:
- **Binary semaphore** use cases are the simplest; in these cases the semaphore is used exactly the same as a lock around a critical section; the initial value here **must** be set to `1`
- Semaphores can also be used as an **ordering primitive** similar to how condition variables would be used; ex. a parent may create a child thread and then call `wait()` while the child calls `post()`; the initial value here must be set to `0`
	- Consider classic problem of ordering statements `a1,a2,b1,b2` where we want `a1` before `b2` and `b1` before `a2`, then we need two semaphores each initialized to $0$, with `post()` called after `a1/b1` and `wait()` before `a2/b2`
		- Note here, there is no requirement for `post()` to only be called after `wait()`
- When using semaphores for **producer-consumer** (bounded buffer) problems, recall the need for multiple condition variables (in this analogue, multiple semaphores)
	- Note, these use cases also require binary semaphores to be used as locks (similar using both condition variables and locks), however the order is **reversed** here; the **binary semaphores must be wrapped by the condition semaphores**
		- Otherwise we will have **deadlock** wherein a thread that blocks on `wait()` on a condition semaphore goes to sleep but still holds the lock semaphore - ultimately all threads that `wait()` on this lock will sleep simultaneously!
	- The semaphore to represent a **filled** (meaning nonempty) buffer must be initialized to `0`
		- Then `wait()` is called on this semaphore from the **consumer** before attempting the binary semaphore, and `post()` is called from the **producer** after adding an element and releasing the binary semaphore
	- The semaphore to represent an **empty** buffer must be initialized to `n` where $n=\text{MAX buffer size}$
		- Then `wait()` called on this semaphore from the **producer** before attempting the binary semaphore, and `post()` from the **consumer** after taking an element and releasing the binary semaphore
	- Consider more abstract examples of deadlocks, like **Dijkstra's Dining Philosophers** problem!
- Semaphores can also be used for **thread throttling** where we want to limit the number of threads that can be running one section of code at once; to do so, we set the initial value to the **maximum number of threads** that we want to allow

Lastly, consider the motivation of critical sections that **only read** the shared data but do not write - then there is no need to ensure mutual exclusivity between threads on this critical section
- Semaphores can be used to implement a **reader-writer lock** wherein any number of threads can hold the read lock while only one thread can hold the write lock (the write lock is just a normal lock in that regard)
	- The key implementation detail is that the **first reader** thread must acquire both the read and write lock, but hold only the write lock (until last reader completion) to allow **any number of readers** but **no writers** while readers are running

```
typedef struct _rwlock_t {
	sem_t readlock; // binary semaphore (basic lock)
	sem_t writelock; // allow ONE writer / MANY readers
	int readers; // #readers in critical section
} rwlock_t;

void rwlock_init(rwlock_t *rw) {
	rw->readers = 0;
	sem_init(&rw->readlock, 0, 1);
	sem_init(&rw->writelock, 0, 1);
}

void rwlock_acquire_readlock(rwlock_t *rw) {
	sem_wait(&rw->readlock); // wait for readlock just to ensure mutual exclusion of below code
	rw->readers++;
	if (rw->readers == 1) // first reader gets writelock
		sem_wait(&rw->writelock);
	sem_post(&rw->readlock); // release readlock so other readers can call acquire_readlock
}

void rwlock_release_readlock(rwlock_t *rw) {
	sem_wait(&rw->readlock); // wait for readlock just to ensure mutual exclusion of below code
	rw->readers--;
	if (rw->readers == 0) // the last remaining reader lets writelock go
		sem_post(&rw->writelock);
	sem_post(&rw->readlock); // release readlock so other readers can call acquire_readlock
}

void rwlock_acquire_writelock(rwlock_t *rw) {
	sem_wait(&rw->writelock); // basic binary lock impl
}

void rwlock_release_writelock(rwlock_t *rw) {
	sem_post(&rw->writelock); // basic binary lock impl
}
```

Note, this implementation of the reader-writer lock is unfair to writers; malicious or errant readers can monopolize the write lock.
# <u>32: Concurrency Bugs</u>
Non-deadlock bugs:
- **Atomicity Violation:** failure to ensure atomicity for a code region which must be executed atomically to prevent data races, etc.; these bugs are simply resolved by using locks (or semaphores as locks)
- **Order Violation:** the desired order between memory accesses is not enforced during execution (in context of interleaved thread execution determined by the scheduler); these bugs are resolved by using condition variables (or semaphores)

**Deadlock:** when one thread is waiting on another which in turn is waiting for the first thread; ex. with two locks held by each of two threads respectively, thread one takes lock one and waits for lock two, which is held by thread two which waits for lock one

**All four** conditions below are required for deadlock to occur; thus preventing even **one** of them will suffice:
1. **Mutual Exclusion:** threads must claim exclusive control of resources that they require (ex. a lock)
	- Deadlock can be prevented by removing the need for mutual exclusion (ie. no locks, no waits), but since our code has critical sections the only way to do this would be to use atomic hardware primitives like compare-and-swap, etc.
2. **Hold-and-Wait:** threads wait for additional resources that they require after acquiring the resources mentioned above
	- This can be prevented by doing **all lock acquisition at once** and only when wrapped in yet another lock specifically to make this set of acquisitions run in mutual exclusivity; however, this reduces the concurrency benefits of threading
3. **No Preemption:** resources cannot be forcibly removed from the threads holding them
	- Preemption of this form can be achieved through a `try_lock()` call which returns an error code upon failing to acquire a lock rather than sleeping and blocking, however this approach is relatively wasteful and can **livelock** 
		- This allows the calling thread to relinquish any other locks that it acquired before this attempt, and simply loop back to the top of the routine to try it all again (scheduling may then allow other threads to acquire the locks)
4. **Circular Wait:** there is a circular chain of threads that hold resources being waited on by the next thread in the chain
	- Then by definition this is not a concern if threads do not have **all** the resources they need shared between them; if one thread needs two locks but thread two only needs one of them, no deadlock can occur between them
	- The **best** way to prevent deadlocks is to ensure there is no possibility of circular waiting; this is achieved by providing **total** or **partial ordering** on lock acquisition (ex. `lock_1` is acquired before `lock_2` always and in all threads)
# <u>37: Hard Disk Drives</u>
Hard disk drives consist of a large number of **sectors**, which are usually $512$-byte (nowadays $4$KB) blocks that can read from or written to; we view the disk address space as an array of $n$ of these sectors
- A **single sector access is atomic**, though modern hard disks often read and write in $4$KB sizes (thus creating the possibility of **torn reads or writes** in case of power outages or errors)
- It is also commonly assumed that accessing sectors in the address space that are closer together will be faster than those that are far apart, and that **contiguous, sequential** reads or writes are thus the fastest modes of access

Physically, a disk is a **platter** coated with magnetic material to create a **surface** (on one or both sides); bits are stored persistently via magnetic charges which do not depend on electricity
- The platters are bound together on a **spindle** which is connected to an electric motor that spins the platters around at a **constant rate**, often measured in rotations per minute (modern drives run at $7200-15000$ RPM; **dimensional analysis** can easily translate this to time per rotation)
- Data is encoded in concentric circles of sectors called **tracks**; with millions of tracks per surface, mere micrometers apart from each other
- There is a **disk arm** and **disk head** per surface; both are electrically powered and move between the tracks of their respective surface

Single-track latency mostly comes from **rotation delay**, the time it takes the disk to rotate to the desired sector so that the arm can access it
- With several tracks there is additional latency from **seek** time as the arm and head move to the correct track (done in parallel with rotation)
- Finally there is the **transfer** time itself, which is when the requisite data is read or written to the sector
- Lastly, the disk often comes with a small bit of memory appended to use as a cache called a **track buffer**; ex. on write the disk may immediately report success while actually holding the data in the track buffer and doing a larger, more efficient **writethrough** later (dangerous!)
$$
T_{io}=T_{seek}+T_{rotation}+T_{transfer}
$$
- From the equation above, we can see that sequential accesses are often much faster (than random accesses) because they require much fewer seeks and incur less rotation delay, and thus are largely bound by transfer speeds which are much faster than seeks and rotation delays!

Due to the high cost of io, the kernel plays a role in **io scheduling** for the disk; unlike with process scheduling, we can make a good guess at how long a disk request will take by estimating the seek time and rotational delay of a request, and enacting principles reminiscent of SJF
- Shortest-seek-first (SSF); theoretically optimal but not all disks provide the kernel with the geometry of the disk needed to determine this, and it can lead to **starvation** if requests for current or close tracks keep coming in and delaying requests to distant tracks
- Nearest-block-first (NBF); simply schedules the accesses for the nearest block to current, but this can still lead to starvation
- SCAN or **elevator** algorithms; these cyclically sweep through all tracks and service any requests in order of their addresses; this resolves starvation but ignores the principles of shortest job first!
- Shortest-positioning-time-first (SPTF); this takes into account **both** seek time and rotation delay, but again this requires constant stream of data on geometry of the disk and is thus usually done by firmware on small controllers integrated with the disk
# <u>39: Files and Directories</u>
**File:** a linear array of bytes which can be read from or written to
- Files have low-level names (usually simply numbers) that are abstracted from the ultimate user, known as **inode numbers**
**Directory:** a list of pairs mapping user-readable names to inode numbers
- Directories themselves have inode numbers, and can hold other directories
- The directory **tree** starts at a **root directory** (in Linux usually named `/`) and uses some **separator** (also `/`) to name subsequent sub-directories or files
	- The **absolute path** to a file or directory is the entire permutation of directories and separators that lead to it; a **relative path** starts at the current directory

Creating a **new file** returns a **file descriptor**; a private per-process number that identifies the file
- The per-process file descriptor table holds a mapping of numbers to pointers to a global **open file table** managed by the kernel
	- Each entry in the open file table has a **reference count** (number of processes sharing this reference), **offset**, and an **inode number**
	- Processes that are created with `fork()` or `dup()` initially have the same numbers in their per-process tables and map to the same entries in the open file table (with incremented reference count)
	- Separate syscalls that return file descriptors will create new entries in the open file table; this allows multiple processes to interact with files while having their own offsets (but this could lead to any number of problems with overwritten data, etc.)

Beyond file access, a filesystem stores information about each file that it stores called **metadata**; this is accessed by the `fstat("pathname")` syscall
- Metadata is stored in a data structure called an **inode** (referenced by the aforementioned inode number), and it holds information including size in bytes, relevant disk blocks, ownership information, permission and control bits, number of hard links (does not track soft links), etc.
- A **hard link** creates a new user-readable name for an inode number; thus to delete a file we use the `unlink()` syscall on existing hard links until the **link count** in the inode is zero, at which point the filesystem clears the whole inode and instructs the disk to mark the relevant data blocks for reuse
- A **symbolic link** (soft link, or symlink) creates a new type of file called a **link file** which holds the absolute path of the linked-to file as the data, but creates the possibility of a **dangling link** if the original file at the path is deleted

When a **new directory** is created, it automatically creates two hard links `.` and `..`; to the current directory and parent directory inodes, respectively
- Additional hard links cannot be created for directories to prevent the possibility of a **cycle** in the directory tree
# <u>40: Filesystem Implementation</u>
The filesystem divides the disk into **blocks**, generally of $4$KB size (other implementations exist, some using varying sizes); this the logical interface for how the filesystem sees the disk, compared to the disk itself with sectors etc.

The majority of the data on a disk will usually be user data, these **data blocks** (dblocks) are known as the **data region** of the disk
- To store the metadata of the files in the data region, the filesystem uses a data structure called an **inode** (index node); before the data region some space is reserved on disk for an **inode table** which is simply an array of on-disk inodes held in **inode blocks** (iblocks)
- Before the inode table, we store **bitmaps** which indicate which blocks are free and which are used; one block is dedicated to an inode bitmap, and another to a data bitmap (consider each block is $4096\cdot 8=32000$ bits)
- Finally the very first block is reserved for the **superblock**, which contains information about the filesystem; the type of system, number of inodes and data blocks, location of the inode table, etc. (the kernel loads the superblock into memory and uses it when mounting the filesystem)

Determining the **sector** of an inode may then look like:
$$
\begin{align}
\text{blockAmount}&=(\text{inumber}\cdot \text{sizeof(inode)}) \;/\; \text{blockSize} \\
\text{sectorNumber}&=((\text{blockAmount}\cdot \text{blockSize})+\text{inodeTableAddr)} \;/\; \text{sectorSize}
\end{align}
$$
An **empty file** would still take up space on disk for its inode (and thus the inode bitmap), but no space for data blocks.

Recall, inside inodes is the **metadata** of the files they represent:
- One of the largest things in some inodes is a set of **direct disk pointers** each of which refers to a disk block that belongs to that file; however, such an approach is not useable in cases where the **file is bigger than the block size multiplied by the number of direct pointers** in the inode
- Using multi-level indexing with **indirect disk pointers** is more flexible; each indirect pointer (the first one being in the inode) points to a data block which in turn holds more pointers and ultimately direct pointers; this can be extended into many levels of pointing but usually only a few levels are necessary:
	- Recall data blocks are $4$KB, then if we assume disk pointers are $4$B each indirect pointer can point to a block that holds $1024$ pointers; thus ex. $12$ direct pointers per inode and $n$ levels of indirect pointers allow us to expand the maximum possible file size to $(12+1024^n)\cdot 4$ KB
	- Alternatives to multi-level indexing include an **extent** parameter approach wherein each inode has an associated length, but this requires contiguous blocks to use; and a **linked list** approach where each block holds a disk pointer to the next one, but performance impact of reads and seeks is clear
		- The extent approach requires the least metadata and can access the $n^{\text{th}}$ block easily (simply calculate location), but is inflexible when new data blocks need to be allocated for a file (requires multiple extents)
		- Linked lists are good for appending but poor for random accesses (require $n$ link traversals to get to the $n^{\text{th}}$ block)

**Directories are files** as well; recall, directories store tuples mapping user-facing filenames to inode numbers (among other things); thus for each such record there is string and numeric data in the data blocks that hold this directory
- Each entry has an inode number, **record length** (the total bytes for the name plus any left over space), **string length** (the actual length of the name), and finally the name of the entry
- Deleting a file (ie. calling `unlink()`) may leave an empty space in the middle of the blocks holding the directory


**Access paths** describe all the io operations necessary to actually read from and write to disk:
- On an **open** for a given path, the filesystem must traverse the full absolute path until it gets to the desired file; **all traversals begin in the root directory**, first reading its inode which has a **well-known inode number**
	- Once the contents of the first inode block (indexed by the above inode number) are read in, the filesystem now has pointers to data blocks with the contents of the root directory (recall, these contents are a map of user-facing names to inode numbers) which it reads to find the inode to the next part of the path
	- Then, the filesystem recursively searches the contents of the data blocks pointed to by each inode until it reaches the ultimate desired file in the path; the final inode is put into memory and a file descriptor is returned to the calling process (after the global open file table is updated, etc.)
	- Note, the amount of io generated by the open is **proportional to the length of the pathname**
- Once opened, **reads** on the file will do **three io operations**; read the inode, read the data blocks, and lastly update the inode's last accessed time; the kernel will also update the open file table entry's offset, etc.; note that **reads do not require checks of the bitmaps** as we are not doing any allocation
- When **writing**, we may need to allocate new inode and data blocks
	- To **create** an empty file we need to update to the inode bitmap, then update the containing directory data, then create a new inode block for the new file
	- Subsequent **writes** to an initially empty file then do up to **five io operations**; read the inode, read and update the data bitmap to allocate new data blocks, write the data blocks, and lastly update the inode's last changed time

To improve performance (reduce io operations to the relatively slow disk), kernels may do:
- Caching important and often-accessed blocks in memory
- Write buffering, batching, and specific scheduling
# <u>41: Locality and the Fast Filesystem</u>
Early filesystems were not optimized for the fact that they were disks rather than RAM
- Data blocks may be written far from inodes which would incur long seeks for the very common operation of reading an inode and then its pointed to data blocks
- Free space management was done in a way that lead to **external fragmentation**, which prevented sequential accesses (the most efficient way to access disk)

Newer filesystems attempt to be **disk aware** while maintaining the same interface as the original UNIX implementation; ex. the Fast File System (FFS):
- Older disks exposed details that allowed a filesystem to separate the address space into **cylinder groups** and place data in ways that minimize seek times
	- Ex. directories in emptiest cylinders so that their files could be placed in same cylinder, inodes in same cylinder as their data blocks, etc.
	- Each group has a copy of the one super block for reliability, as well as its own inode bitmap and data bitmap, inode table and data blocks (each group is effectively its own filesystem!)
- Newer disks do not expose these details and so they just put contiguous blocks into **block groups**

Overall, performance gains are gotten from placing related items close together and unrelated items far apart:
- Cylinder groups with a **low number of allocated directories** (to balance directories across groups) and a **high number of free inode blocks** (to subsequently be able to allocate a bunch of files) are used for **new directories** (inode and data)
- For files, inodes are put into the **same groups as their pointed to data blocks** (preventing long seeks between fetching inode and data), and files are placed in the **same group as their containing directory**
	- Exceptions are made for **large files**; ex. only the direct pointers of an inode have their data blocks in the same group, the remainder in other groups (to prevent one single large file from dominating a group); in practice the maximum chunk size can be chosen with other considerations in mind as well 

Other performance concerns:
- **Internal fragmentation** caused by a block size of $4$KB while most files are smaller than that; solved by having sub-blocks (ex. $512$B to match sector sizes) or by write buffering until a certain size is reached
- **Parameterization** of the disk is sometimes necessary to decide on how to stagger blocks, ie. prevent a full rotation when doing sequential reads and requesting the next block too late (disk already spun past it)
# <u>42: Crash Consistency and Journaling</u>
**Crash scenarios** on write, where **one** of three structures is updated:
- Bitmaps and inode are not updated, data block is; this is a problem for the user (who now cannot access this data) but the filesystem regards it as **consistent**
- Bitmaps and data block are not updated, inode is; the inode will have pointers to data blocks which have not been updated and thus will point to **garbage data**, and this leads to filesystem **inconsistency** as the bitmaps will not indicate that data blocks have been allocated while the inode disagrees, other inodes may then point 
- Bitmaps are updated, inode and data blocks are not; this will result in a **space leak** due to inconsistency wherein a block is indicated as allocated but not pointed to by any inodes and thus cannot be used
Crash scenarios on write, where **two** of three structures are updated:
- Bitmaps and inode are updated, data block is not; this is again a problem for the user (who will access garbage data) but the filesystem regards it as consistent
- Bitmaps are not updated, inode and data block are; the user may be able to access the data, but this is a filesystem inconsistency as the bitmaps and inode disagree and multiple inodes may be able to point to the same data blocks (and rewrite them, after which the original inode will point to **garbage data**)
- Bitmaps and data block are updated, inode is not; this will result in a **space leak** due to inconsistency wherein a block is indicated as allocated but not pointed to by any inodes and thus cannot be used

Like writes above, several filesystem operations require multiple steps and a (relatively) long time; one solution is the infamous filesystem checker tool **fsck**
- We can let crash inconsistencies happen but and then attempt to patch them on reboot with fsck; note this **only concerns itself with inconsistencies**, so scenarios where bitmap and inode agree but a data block was never written are not covered (user will access garbage data)
- Fsck runs **before the filesystem is mounted**, where it performs many, many thorough sanity checks and validations
Of course fsck is **very slow**; often too slow to justify when a crash may have impacted only a few blocks!


**Journaling** attempts to solve crash inconsistency before the crash:
- The journal is held in a well-known spot on disk
- Logs are marked by transaction start and end blocks; between which is either the entirety of the data to be written, or a more compact logical representation
	- The transaction start block, and all other data is written first (this **ordering** is enforced; this is a form of **write barrier** which guarantees that all preceding writes are complete before proceeding)
	- Only once this all complete, the transaction end block is written **atomically** (by being less than $512$ bytes) to **commit** the journal entry; this prevents issues with **inconsistencies in the journal itself** in case of a crash
- Once the log is written to disk, the operation of updating the intended disk locations with the data from the journal is called **checkpointing**
	- Crashes can happen at any time during checkpointing; the journal ensures that on reboot we can do **replay logging** and restore consistency
- Free checkpointed transactions so the journal disk space can be reused; usually by simply marking the oldest and newest non-checkpointed transactions (everything else is free) and thus enabling a circular log of sorts
	- If certain transactions have been checkpointed but not freed before a crash, there will be some redundant writes but this is only a performance issue

There is an obvious performance impact to journaling; recovery is now faster than fsck since we only replay some transactions rather than parsing the whole disk, but normal operation of the filesystem is slower since every write now has to be done twice
- This can be helped by doing **metadata journaling** only (ie. bitmaps and inodes, no data blocks) rather than full data journaling; in this case the **data block has to be written to its intended disk location first** before the metadata is committed in the journal
	- If this order is reversed, a crash before the data is written can lead to metadata in the journal which is consistent but points to garbage
	- Metadata journaling **performs better than full journaling** during normal operation, recovery, in terms of storage, etc. but it **provides less protection for the data specifically** (same protection for metadata)
# <u>44: Flash SSDs</u>
**Solid-state storage devices** (SSDs) have no moving mechanical parts unlike hard disks, but rather are made of transistors (specifically **NAND flash**) same as memory; however, SSDs persist data without power (unlike other transistor circuitry)
- Bits are represented by the amount of charge trapped in a transistor; various cells exist which can encode up to $3$ bits per cell depending on charge
	- These cells are prone to **wear** and electrical **disturbance**, but still less physical problems than the mechanics of hard disks
- SSDs are organized into **banks** or planes of a large number of cells, banks are divided into **erase blocks** typically of size $128-256$KB within which are **pages** which are typically $4$KB
- Operations on SSDs:
	- A client can **read** any page by providing a page number; this operation is very fast regardless of the location on the device and of the location of the previous request, thus unlike a hard disk it is a **random access** device
	- A client can **write** to a page by a combination of **two commands**; **erase** clears an entire block by setting all bits to $1$ and is a relatively expensive command (has to affect all bits in an entire block) and **program** sets the desired bits in a block to $0$ thus creating the desired data
		- After being programmed, a page **cannot be programmed again** without the entire block being erased first (ie. each page can only be programmed **once** before requiring an erase, which in turn can only be done on an entire block)

A **page** can be in **one of four states**:
- i; **initial** state (cannot be read or written to)
- E; **erased** (cannot be read, but can be written to; only erased pages can be written to)
- V; **valid** (a valid page that has been written to; it can be read but cannot be written to again)
- +; **trimmed** (the page's mapping from logical block to page number has been removed by an explicit `trim()` request)

The **flash translation layer** (FTL) exposes a hard-disk-like interface (logical blocks, etc.) and then efficiently translates these operations into SSD operations
- This layer uses an on-device chip and SRAM cache
- This layer performs **wear leveling** across blocks to amortize wear more evenly across the SSD
- FTLs **do not do direct mapping** since the kernel is unaware of SSD internals; this would lead to **write amplification** (ex. writes would have to read in entire blocks, then erase, then program new data and read in data) and lead to possible wear (ex. frequent rewriting of metadata)
- Instead, FTLs are **log structured**:
	- Kernel addresses are translated into optimized addresses on the SSD; an in-memory (on SSD) **mapping table** maintains the mapping of kernel addresses to SSD addresses; this allows for reads while also allowing the SSD chip to optimize writes (write to empty pages in blocks that have already been cleared)
		- These mapping tables can become large for larger SSDs; there are some hybrid schemes wherein the table holds **block-level** mappings and page-level mappings, caching, dedicated write blocks, etc.
	- When the kernel overwrites data at the logical addresses it has, that data gets mapped to new SSD pages (recall, pages cannot be rewritten without clearing entire block) and the old pages become garbage which periodically has to be **garbage collected** 
		- Garbage collection itself would also involve clearing entire blocks and **moving the remaining valid pages** from those blocks to newer blocks still