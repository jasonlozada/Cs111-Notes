## LECTURE 2 Slides Cont.
### Service Delivery via Sys Calls
Force an entry into the OS
- Parameters/returns similar to subroutine
- Implementation is in shared/trusted kernel
- Advantages:
    1. Able to allocate/use new priviledged resources.
    2. Able to share/communicate with other processes
- Disadvantages:
    1. 100x-1000x slower than subroutine calls

### Providing Services via the kernel
Primarily functions that require privilege:
1. Privileged instructions (ex: interrupts, I/O)
2. Allocation of physical resources (ex: memory)
3. Ensuring process privacy and containment
4. Ensuring the integirty of critical resource

Some operations may be out-sources (system daemons, server processes) and some plug-ins may be less trusted (i.e. Device drivers, file systems, network protocols)


### System Services Layer
Layers | |
--- | --- 
1 | (user and system) apps 
2 | Operative system services and middleware services |
 **Application Binary Interface** | ---
4 | general libraries |
5 | drivers, Operating System kernel |
**Instruction Set Architecture** | ---
6 | devices, priviledged instruction set, general instruction set


### System Outside the Kernel 
Not all trusted code must be in the kernel as it may not need to access kernel data structures and may not need to execute priviledged instructions

Some are actually somewhat privileged processes:
1. Login can create/set user credentials
2. Some can directly execute I/O operations

Some are merely trusted:
1. sendmail is trsuted to properly label messages
2. NFS server is trusted to honor access control data

### Service Delivery via Messages
The systen exchange messages with a server vie syscalls. Parameters in request, returns in response. 

Some advantages include:
1. Server can be anywhere.
2. Server can be highly scalable and available
3. Service can be implemented in user-mode code

Some disadvantages include:
1. 1000x-100,000x slowe rthan subroutine
2. Limited ability to operaton on process resources

### System Services via Middleware
Middleware is between application and library. It is software that is a key part of the application platform, but part of the OS:
1. Database, pub.sub messaging system
2. Apache, Nginx
3. Hadoop, Zookeeper, Beowulf, OpenStack
4. Cassandra, RAMCloud, Ceph, Gluster

Kernel code is very expensive and dangerous since user-mode code:
- is easier to build, test and debug
- is much more portable
- can crash and be restarted

# LECTURE 3: OS Interfaces
The OS is meant to support other programs via its abstract services. It is usually intended to be very general and supporting many different programs. Interface are required between the OS and other programs to offer general services.

### Interfaces: APIs
**Application Program Interfaces**. APIs help you write programs for your OS. This is a source level interface specifying: 
- include files, data types, constants 
- macros, routins and their parameters.

It is a basis for software portability as it recompiles program for the desired architecture, create linkage edit with OS-specific libraries, and results in binary runs on that architecture and OS

An API complian program will compile & run on any compliant system. APIs are primarily for programmers.

Without portability, it would be difficult to do CS. 

### Interfaces: ABIs
**Application Binary Interfaces**. A binary interface that specifies dynamically loadable libraries (DLLs), data formats, calling sequences, and linkage conventions. This binds an API to a hardware architecture. ABIs help you install binaries on your OS.

This is a basis for binary compatibility. One binary serves all customers for the hardware (ex: x86 Linux/BSD/MacOS/Solaris/..)

An ABI compliant programwill run (unmodified) on any copmliant system. 

They are primarily for users as it doesn't matter what machine they have as long as it has a similar CPU.

### Libraries and Interfaces
Normal Libraries (shared and otherwise) are accessed throuh an API:
- Source-level definitions of how to access the library
- Redily portable between different machines

Dynamically loadable libraries are called through an API but the dynamic loading mechanism is ABI-specific. However, there are issues of word length, stack format, linkages, etc.

### Interface and Interoperability
Strong, stable interfaces are crucial to allowing programs to operating together and allowing OS evolution as you don't want an OS upgrade to break your existing programs (it also means the interface between the OS and those programs better not change).

Big comapnies try not to change their APIs and ABIs because they don't want users to complain about their programs breaking when upgrading their OS.

### Side Effects
A *side effect* occurs when an action on one object has non-obvious consequences such as effects not specified by interfaces or even to other objects. These are often due to shared state between seemingly independent modules and functions.

Side ffects lead to unexpected behaviors and the resulting bugs can be hard to find. **TIP** Avoid ALL side effects in complex systems.

### Abstractions
Many things an OS handles are complex due to varieties of hardware, software, configurations. The operating system creates, manages, and exports such abstractions.

### Critical OS Abstractions
THe OS provides some core abstractions that our computational model relies on and build others on top of those (ex: Memory abstractions, processor abstractions, communications abstractions)

### Abstractions of Memory
Many resources used by programs and people relate to data storage (i.e. variables, chunks of allocated memory, files, database records, messages to be sent and received). They all have some similar properties: you **read** and you **write** them.

### Some Complicating Factors
1. Persistent (things you want to stay: file system) vs. Transient memory (when machine is off, you don't expect to see the same values of RAM)
2. Size of memory operations
    - Size the user/app wants to work with
    - Size the physical device actually works with
3. Coherence and atomicity
    - Coherence: wrting in location x and then reading that location, there should be the same value that was written when it is read.
    - Atomicity: All or none info
4. Latency
5. Same abstraction might be implemented with many different physical devices (possibily of very different types)


### Where Do the Conplications Come From?
