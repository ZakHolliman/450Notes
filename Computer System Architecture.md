From lecture on 2/9/2023

## Overview
* CPU
* Interrupts
* Memory Hierarchy
* I/O Devices
* Buses
* Single and Multiprocessor Systems

### CPU



### Multiprocessors
Multiprocessors are a system 


There are also **Clustered Systems**, 

### Multiprogramming
The OS keeps several processes in memory at the same time. The OS scheduler picks one program to run at a time, running through the queue until it's empty. This ensures that the computer is being used at its maximum capacity for as often as possible.

The next extension of this is *multitasking*, which allows a user to perform more than one computer task at a time. The OS is able to keep track of where you are in these tasks and can continue without loss of information.

### Dual-Mode
Most OS incorporate a *Dual-Mode* system that allows a user to use a *kernal mode* and a *user mode*. The Kernal Mode allows more bare metal changes to the computer

When a system call is made, the computer switches from User mode to Kernal mode with the *mode bit*.

### System Calls
System calls allow a user to essentially ask the OS to perform a task that is reserved for lower level tasks that are usually reserved only for the OS to make.

A System Call is a type of IRQ. When the System Call is called, the ISR is passed through to the OS and the computer enters Kernal Mode. The System Call is evaluated and executed, and then the computer switches back into User Mode.

An example of a System Call would be

```C
#include <unistd.h>
ssize_t read(int fd, const void *buffer, size_t nbyte);
```

### Timers
An OS maintains control over the CPU with a *timer*. A process cannot keep the CPU for as long as it needs at one time, as it needs to be able to let other processes take control of it. As such, the OS uses Timers to set an interrupt to a process to make it swap the use of the CPU.