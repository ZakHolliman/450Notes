From class on Feb 14 2023

Early computers allowed only one program to be run at a time, but over time we've developed the capability for them to execute many programs at the same time. We calls these *processes*.

## The Process (3.1.1)
A `program` becomes a `process` when an `executable file is loaded into memory`. A process is a program in execution. When a program starts to run, OS keep its status in a process table until it termination.

The memory layout of a process is typically divided into four sections
* Text section - The executable code (Fixed size)
* Data Section - Global variables (Fixed size)
* Heap - Used for dynamic memory allocation (Can change size)
* Stack section - Temporary data storage for local variables when a function is called  (Changes size during execution)

For the purposes of this example, we'll assume that we only have `one CPU` in our system.

Multi programming - Several jobs are loaded in a memory operating system simulate Pseudo parallelism

OS schedule the CPU times for processes by switches from one process to another based on the scheduling algorithm and process state.

The order of these in data goes (starting at address 0)
* Text -> Data -> Heap -> Storage in between Heap and Stack -> Stack

Although the Stack and Heap grow towards each other, the OS ensures that they do not overlap.

There is only one CPU, but there can be many *Processes* that we want to run concurrently. This is done by Scheduling

## The Process Model
We can consider processes as two different points of view
* *Model* - Multi programming, and processes share resources
* *Conceptual Model* - Each process has its own virtual CPU and RAM

A *Program* and a *Process* are separate entities. *Programs* are passive by default, and they only become active *Processes* when they are executed and their data is loaded into memory.

With the CPU switching back and forth among the processes, a process can hold CPU during its time term

A *Time Term* is a unit of time that the system designates to a process for it to be able to run for whenever it is set to run. These time terms may not be uniform in length for every process. The Time Term length could vary based on if the job is I/O bounded, CPU bounded, or has higher priority.

## Process Creation (3.3.1)
There are several ways that an OS can create a process, here are some of the most common
* System Initialization - When an OS is booted, several processes are created and run concurrently
* A running process creates a process by a system call - Running processes can request a system call to create one or more new processes
* A system user can create a process by executing  a program - In interactive systems, a user can start a program by typing a command or clicking an executable
* Initiation of a batch job - Mainframe computer (??? okay lmao)

The way these processes are created varies from system to system
* UNIX - system call `fork` creates a new process
* Windows - system call `CreateProcess` is used

## Process Termination (3.3.2)
There are two types of Process Termination: `Voluntary` and `Involuntary`

A process terminates due to one of the following conditions
* *Normal Exit* - A process finishes its job (`Voluntary`)
* *Error Exit* - The process discovers a fatal error (`Voluntary`)
* *Fatal Error* - Error caused by the process, a process tries to modify memory where other processes are located (`Involuntary`)
* *Killed by Another Process* - When a deadlock is discovered, OS terminates a process at a time to resolve the deadlock (`Involuntary`)

## Process State (3.1.2)
A process must be in one of the following states
* *Running State* - A process is currently being executed by using CPU
* *Block State / Waiting State* - The process is waiting for some event to occur such as I/O or receiving a signal
* *Ready State* - A process is ready to use a CPU but it is not currently available

## Process Table
When a process is created, the OS stores its run time information in a process table, also called the `Process Control Block`

The `Process Table` contains information associated with a process

## Processes with Multiple Threads
Most modern operating systems have extended the process concept to allow a process to have *multiple threads* of execution, and thus to perform more than one task at a time.

On systems that support threads, the process table is expanded to include information for each thread.

## Process Scheduling
The objective of Multi-programming is to `maximize the usage we get out of the CPU`. When a CPU becomes available, the `Short Term Scheduler` selects a process from the ready queue and hands it off to the CPU.

There are two types of queues which holds pointers to process tables
* `Ready Queue` - Holds pointers to process tables for `processes in ready state`
* `Wait Queue` - Holds pointers to processes `waiting for some kind of event`

These queues are generally `stored as linked lists`.  The header contains a pointer to the first process table in the list, and each process table includes a pointer field that points to the next process in the queue.

Once a process is given a CPU and is executing, several things can happen.
* Process can `issue an I/O request` and be placed in an I/O wait queue
* It can `create a new child process` and be placed in a wait queue while it `waits for the child's termination`
* It can be `removed forcibly` from the core as a result of `an interrupt` or having its `Time Term expire`

A process migrates to the `ready queue` from various `wait queues` throughout its lifetime. This is where the `Short-Term Scheduler` comes in, as it selects from the processes that are in a ready queue and `assigns a CPU to them`.

`Interrupts` can also cause the OS to switch processes to run a kernel routine. When it does this, it's called a `Context Switch`. During this, the OS `creates a state save` of its current process, and then `switches to a state load` of the process that the Interrupt called for. The `CPU must be idle` during a context switch.

A `process can create many new processes`, which in turn can also `create their own processes`, and so on and so forth, creating what is called a `Process Tree`.

In Linux, `once the system has booted`, the `systemd` process is created with `pid = 1` and this becomes the `root parent process of the entire system`.

Linux systems can use the `pstree` command, which `displays a tree of all processes in the system`.

```
$ pstree -p
```

### Process Creation
When a parent creates a new child process, the child will need resources in order to run. The parent will either `allocate new resources` for the child to use, or it will `partition its own resources` and split some off to the child for it to use.

When a process creates a new process, there are two possibilities for what can happen
* The parent `continues to execute concurrently` with its children by `waitpid()` system call
* The parent `waits until some or all of its children have terminated` by `wait()` system call

When a child is created, it can either be a `duplicate` of its parent, and `inherit all of its data`, or it can be a `new program` and have its `own separate data`.


### Process Termination
A process `terminates` by calling the `exit()` system call. At that point, the process will return a status value to its waiting parent. All of the `resources are then deallocated and reclaimed` by the OS.

### Zombie Processes
A `Zombie Process` is a process that `has terminated`, but who's parent has `not called wait()` yet.

### Orphan Processes
If a `parent process terminates before a child` process, the child becomes `Orphaned` and the `root process systemd will become its parent`

