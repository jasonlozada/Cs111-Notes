## LECTURE 1 SLIDES Cont.

### Instruction Set Architectures
The set of instructions supported by a computer
1. Which bit patterns correspond to what operations

There are many different ISAs (all incompatible)
- Different word/bus widths (8, 16, 32, 64 bit)
    - A bus, in computing and digital technology, is an electronic pathway through which data can be transferred. This pathway uses signals that move at different speeds and are sent through different channels to communicate information between components within a computer or network.

- Different features (low power, DSPs, floating point)
– Different design philosophies (RISC vs. CISC)
- Competitive reasons (x86, PowerPC, Apple Silicon)

They usually come in families
- Newer models add features (e.g., Pentium vs. 386)
- But remain upwards-compatible with older models
    - A program written for an ISA will run on any compliant CPU

### Priviliedged vs General Instructions
Most ISAs divide the instruction set into **privileged** vs. **general** 
#### **General**
 Any code running on the machine can execute general instructions.
#### **Priviledged**
Processor must be put into a special mode to
execute privileged instructions. It is usually on in this mode when the OS is running. Note that priviledge instructions do things that are *dangerous*.

### Platforms
ISA doesn't completely define a computer. There is functionality beyong user mode instructions like:
1. Interrupt controllers, DMA Controllers
2. Memory management unit, I/O busses
    - I/O devices: Display, disk, network, serial device controllers
3. BIOS, configuration, diagnostic features
4. Multi-processor & interconnect support.

These variations are called "platforms" aka the platforim in which the OS must run. Note that there are lots of them.


### Probability to Multiple ISAs
A successful OS will run on many ISAs. 
- Some customers can't choose their ISA
- If you don't support it, you can't sell to them

This implies that the OS will abstract the ISA

There are minimal assumptions about specific Hardware (HW):
- General frameworks are HW independent like file systems, protocols, processes, etc.
- HW Assumptions isolated to specific modules like context switching, I/O, memory management
- Careful use of types like word length, sign extension, byte order, alignment


So, how can an OS manufacturer distrubute to all these different ISAs and platforms?

### Binary Distribution Model

Binary is the derivative of source (OS is written in source). But a source distribution must be compiled. This means that a binary distrbbution is ready to run.

OSes are usually distributed in binary. There is one or more binary distributions per ISA. 

Binary model for platform support:
- Device drivers can be added, after-market. This can be written and disributed by 3rd parties. The same driver works with many versions of OS.

### Binary Configuration Model
This is good to eliminate manual/static configuration as it enables one distribution to serve all users and improves both ease of use and performance.

There is also an automatic hardware discovery...
- Self-identifying busses: PCI, USB, PCMCIA, EISA, etc.
- Automatically find and load required drivers

AND automatic resource allocation. This allows to eliminate fixed sized resource pools and dynamically (re)allocate resources on demand.

### What Functionality Is In The OS?
As much as necessary, as little as possible since OS code is ***very expensive*** to develop and mantain. 

The functionality muse be in the OS if it:
- requires the use of privileged instructions
- requires the manipulation of OS data structures
- must maintain security, trust, or resource integrity.

The functions should be in libraries if they are a service commonly needed by applications or do not actually have to be implemented inside OS. However, there is also the performance excuse as some things may be faster if done in the OS.


### Conclusion
- Understanding operating system is critical to understanding how computers work
- Operating systems interact directly with the hardare
- Operating systems rely on stable interfaces


# LECTURE 2 Slides: OS Services

### The OS And Abstraction
OS offers abstract versions of resources (as ooposed to actual physical resources). Essentially, the OS implelemnts the abstract resources usinf the physical resources such as proceeses and files.
For processes, these are implemented using the CPU and RAM (physical resources). So when you want to listen to music, the music is the abstraction and the speakers are the physical resources. For files, they are implemented using flash drive (a physical resource).

Abstractions are a bunch of code and takes them into a specific task we want them to do.

Processes are built out of software in the OS, so they are abstractions as they have to interact with low-level systems. 

### Why Abstract Resources
The abstractions are typically simpler and better suited for programmers and users:
1. They are easier to use than original resources. 
    - Ex: you don't need to worry about keeping track of disk interrupts.
2. Comparementalize/encapsulate complexity.
    - Ex: you don't need to be concerned about what other executing code is doing and how to stay out of its way
3. Eliminate behavior that is irrelevant to user.
    - Ex: Hide the slow erase cycle of flash memory.
4. Create more convenient behavior.
    - Ex: Make it look like you have the network interface entirely for your own use.

### Generalizing Abstractions
There are lots of variations in machines' HW and SW. Since there are many different types of abstractions that appear the same, we can convert them into the same abstraction so applications can deail with single common class. Abstraction also involves a common unifying model. For example  (ex: pdfs for printers or SCSO standard for disks, CDs and tapes)

### Common Types of OS Resources

#### Serially Reliable Resources
These are used by multiple clioents, but only one at a time (time multiplexing)
- Require access control to ensure execlusive use
- Require graceful transitions from one user to the next
- Ex: Printing a double-sided document first then printing a colored document next. The switch between these two should be so that the first task should not be carried over to the second task

#### Graceful Transition

A switch that tollay hides the fact that the resource used to belong to someone else. 

- Don’t allow the second user to access the
resource until the first user is finished with it
    - No incomplete operations that finish after the
transition
- Ensure that each subsequent user finds the
resource in “like new” condition
    - No traces of data or state left over from the first user

#### Partitionable Resources

Divided into disjoint pieces for multiple client aka Spatial multiplexing.

- Needs access control to ensure:
    - Containment: you cannot access resources outside
of your partition
    - Privacy: nobody else can access resources in your
partition

















