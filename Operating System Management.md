## Process Management
The OS is responsible for the following tasks in process management
* Creating and deleting user and system processes
* Scheduling processes on the CPUs
* Susepdning and resuming processes
* Providing mechanisms for synchronization
* Providing mechanisms for process communication



## Memory Management
For a program to be executed, it has to be loaded into the Program Memory of the machine, and it must be given an address. This address is then fetched and executed by the CPU during the fetch-execute cycle, and reads the instruction in memory to decode it.

The OS is responsible for the following things for memory mangament.
* Keeping track of which parts of memory are being used and which are free
* Allocating and deallocating memory as needed
* Deciding which processes and data to move between memory spaces.



## File Management
The OS manages files for users and allows them to safely store and managed data in files without loss of information or danger of deleting anything. If a computer has multiple users, it also manages individual user rights to files.

The OS is responsible for the following things for file mangament.
* Creating and deleting files
* Creating and deleting directories
* File manipulation primitives
* Mapping files into mass storage
* Back up files



## Mass-Storage Management
The OS is also responsible for managing storage in things such as HDD, SSD, USB, etc...
* Mounting and unmounting
* Free-space management
* Storage allocation
* Disk scheduling
* Partitioning
* Protection



## Cache Management
The *Cache* is a short-term memory that may be used again soon. Things are normally kept in a main, solid storage system such as an HDD, but this is slow to work with. As a result, for memory that we want to work with often, we transfer it over to a closer, more efficient memory called *Cache memory*. When the OS fetches a piece of information, it first checks the cahce. If the information is there, it grabs it, but if it isn't, it retrieves it from long-term memory, and then stores it in the cache under the assumption that it will be needed again soon.

Cache memory is limited however, and as such must be carefully managed to ensure that the data we are looking for is there, and that we do not go over the limit of the cache.

A cache hit occurs when requested data can be found in the cache, while a cache miss is when it cannot. The OS manages the cache's limited memory in order to ensure increased and steady performance.



## I/O Management
The OS also abstracts away a lot of the interfacing for I/O devices, such as:
* Installing drivers for detected devices
* Acting as a general device driver interface
* Managing memory of I/O devices such as buffering, caching, and spooling
* Issuing commands to devices
* Catching IRQs from devices
* Handling error