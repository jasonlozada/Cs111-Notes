## READINGS
- [Operating System Principles](https://lasr.cs.ucla.edu/reiher/cs111/principles.html)
- [Arpaci-Dusseau Chapter 1](http://pages.cs.wisc.edu/~remzi/OSTEP/dialogue-threeeasy.pdf)
- [Arpaci-Dusseau Chapter 2](http://pages.cs.wisc.edu/~remzi/OSTEP/intro.pdf)
- [Projects to do](https://github.com/remzi-arpacidusseau/ostep-projects)

# Operating System Principles

## 1. Introduction 
- When building increasingly big amounts of increasingly complex software, size and complexity become problems due to some of these factors:

What | Why
--- | ---
Increasingly sophisticated software | typical project will involve multiple orders of code with more magnitude compared to projects 20-30 years ago.
Increasing complexity | Work involved in creating a piece of software is much worse than linear with the number of lines of code involved. It is also very difficult to learn and full understand our software, leading to errors in constructing it.
Systems are now assembled from numerous independently developed and delivered pieces | Small changes in one component often result in failures of other components. As the number of independent interacting components increase, there are more complex emrgent behaviors that would not have been predicted from smaller systems.
Increasingly rich functionaly and interactions with increasingly many other external systems | This makes complete testing combinatorically impossible. The increasing dependency on this software means that failuers will cause more severe problems, for more people, more often.


### What are involved in an Operating System (OS):
- Complex intreactions among many sub-systems
- Numerous asynchrounous interactions and externally originated events
- The sharing of stateful resources among cooperating parallel processes
coordinated actions among heterogenous components in large distributed systems. 

### Musts of an OS:
- Evolve to meet ever-changing requirements
- Be able to run, without error, for years despite the regular failures of all components.
- Portable to virtually all computer architectures and able to function in heterogenous environments (with many different versions of many different implementations, on many different platforms)



## 2. Complexity Management Principle

### 2.1: Layered Structure and Hierarchical Decomposition

Break the components of a operating system down into smaller components so that it can be understood individually

Ex: Hierarchical decomposition is the process of decomposing a system in a top-down fashion. If we wanted to describe the University of California, we might

1. Start by dividing it into the Regents, the Office of the President, and the campuses.
2. The campuses can be divided into ten university campuses (UCLA, UCSB, ...) and three laboratories (Livermore, Los Alamos, ...)
3. One campus (e.g. UCLA) can be divided into administration and schools (Letters and Sciences, Engineering, Medicine, ...)
4. Administration can be divided into departments (personnel, legal, finance, registrar, deans, ...)
5. Each School (e.g. Engineering) can also be divided into departments (Computer Science, Materials, ...)
6. A department can be divided into administrative staff, support staff, and faculty
7. Administrative staff can be divided into responsibility areas (payroll, purchasing, personnel, ...)

**Hierarchical decomposition** is not merely a technique for studying existing systems, but also a principle for designing new systems. 

### 2.2 Modularity and Functional Encapsulation

#### Requirements for an OS (arbitrary decomposition):
- Each group/component (at each level) have a coherent purpose.
- Most functions (for which a particular group is responsible) can be performed entirely within that group/component.
- The union of the groups/component (within each super-group/system) is able to achieve the system's purpose.

### 2.3 Appropriately Abstracted Interfaces and Information Hiding

#### Appropriate Abstraction
An interface that enables a client to specify parameters that are most meaningful to them and easily get the desired results. Ex: A faucet where the user knows whether cold or hot water is being flowed.

#### Poorly Abstracted
An interface that does not provide a client with what they want. Ex: Water faucet in which the user doesn't know whether the water flowing will be hot or cold (the user wants to control whether the water flowing will be hot or cold).

#### Opaque 
An appropriately abstracted interface that does not reveal the underlying implementation. This exhibits good *information hiding* not exposing implementation details that clients did not want to be forced to understand.
ex: The two-valve interface is tightly coupled to a hot- and cold-water supply implementation. This showcases good *information hiding* as the users do not need to know how the faucets flow hot/cold water.


### 2.4 Powerful Abstractions
Virutally all tools and resources in operating systems abstract concepts from the imagination of system architects. 

A powerful abstraction can be applied to many situations suchs as:
- common paradigms (e.g. lock granularity, cache coherency, bottlenecks) that enable us to more easily understand a wide range of phenomena.
- common architectures (e.g. federations of plug-in modules treating all data sources as files) that can be used as fundamental models for new solutions.
- common mechanisms (e.g. registries, remote procedure calls) in terms of which we can visualize and construct solutions

Sufficiently powerful abstractions give us tools to understand, organize and control systems which would otherwise be intractably complex.

### 2.5 Interface Controls
Every system, sub-system, or component is primarily understood in terms of the services it provides and the interfaces through which it provides them. 

An **interface specification** is a contract: a promise to deliver specific results, in specific forms, in response to requests in specific forms. If the service-providing component operates according to the interface specification, and the clients ensure that all use is within the scope of the interface specifications, then the system should work, no matter what changes are made to the individual component implementations.

### 2.6 Progressive Refinement

Software projects that attempt to to grand things, starting from a clean slate, often fail after spending many years without delivering useful results:

- It is very difficult to estimate the work in a large project
- It is very difficult to anticipate the problems in a large project.
- It is very difficult to get the requirements right in a large project
- They often so long that they are obsolete before they are finished
- They often lose support before they are finished.

This is why most modern techonologies embrace **interative or incremental** development:

- Add new functionality in smaller, one feature at a time, projects. It is much easier to identify likely problems (and solutions) in a smaller project, and hence to estimate the required work and likely delivery date. Also the work can be done by a smaller team with better communication, and so is likely to be done more efficiently.
- Rather than pursue large speculative (if you build it they will come) projects, identify specific users with specific needs, and build something that addresses those needs (while moving us in the right direction). Having a specific problem to solve leads to better requirements. Delivering needed functionality to actual users yields creates more value more quickly.
- Deliver new functionality as quickly as possible to get feedback before moving on to the next step. Most software requirements are speculative. The best way to clarify them is to deliver something and get feedback.
- It is much easier to plan the next step after we have data from the previous step. Real performance data or user feedback will guide us towards the most important improvements. If we are going down the wrong path, it is best that we find that out as soon as possible.


## 3. Architectural Paradigms
Over many decades of building ever more powerful operating systems we have found a few very powerful concepts of very broad applicability.

### 3.1 Mechanism/Policy Separation
Mechanisms for managing resources should not dictate or greatly constrain the policies according which those resources are used as the system is likely to be used used for a long  time. This means that resource managers should  be designed in two logical parts:
1. The *mechanisms* that keep track of resources and give/revoke client access to them.
2. A configurable or plug-in policy engine that controls which clients get which resources when.
- Exanple for a good mechanism/policy separation is a **card-key lock system**:
    - each door has a card-key reader and a computer-controlled lock.
    - each user is issued a personal card-key.
    - to get access to a room, a user swipes a card-key past the reader.
    - a controlling computer will lookup the registered owner of the card-key in an access control database in order to decide whether or not that user should be granted access to that door at that time, and (if appropriate) send an unlock signal to the associated lock.
    - **EXPLAINATION**: The physical mechanisms are the card-keys, readers, locks, and controlling computer. The resource allocation policy is represented by rules in the access control data-base, which can be configured to support a wide range of access policies. The mechanisms impose very few constraints (beyond those of the rule language) on who should be allowed to enter which rooms when. And, because the policy is independent from the mechanisms, we could also move to a very different mechanism implementation (e.g. RFID tags), and continue to support the exact same access policies. It should be possible to change either mechanism or policy without greatly impacting the other.


### 3.2 Indirection, Federation, and Deferred Binding
 The most common way to accommodate multiple implementations of similar functionality is with plug-in modules that can be added to the system as needed. Such implementations typically share a few features:

- A common abstraction (class interfaces) to which all plug-in modules adhere.
- The implemntations are not built-in to the OS, but in the OS, but can be accessed indirectly (i.e. pointers)
- The indirection achieved through **federatoin framework** that registers available implementations and enables clients to select the desired implementation and automatically routes all future requests to the selected object/implementation.
- The binding of a client to a particular implementation does not happen when the software is built but rather is deferred until the client actually needs to access a particular resource.
    - The **deferred binding** may go beyond the client's ability to select an implementation at run-time. The implementing module may be dynamically discovered, and dynamically loaded. It need not be known to or loaded into the operating system until a client requests it.

### 3.3 Dynamic Equilibrium
Most complex systems have numerous tunable parameters that can be used to optimize behavior for a given load however this is not enough to ensure that systems are well tuned as:
- Tuning parameters tend to be highly tied to a particular implementation, and proper configuration often requires deep understanding of complex process.
- Loads in large systerms are subject to continous change.
- There is a possiblity to build automated management agents that continious monitor system hevorior but the automations can misinterpret sypmtoms or drive the system into uncotrolled oscillation.

Note that the stability of a complex natural system is the result of a dynamic equilibrium. A **dynamic equilibrium** is where the state of the system is the net result of multiple opposing forces and any external event that disrupts the equilibrium autommaticaly triggers a reaction in the opposite direction.

### 3.4 The Criticality of Data Structures
Data structure designs (and their correctness/coherency requirements) determine: 
1. which operations are fast and which are slow (e.g. because of the number of pointers to be followed)
2. which operations are simple and which are complex (e.g. because of how many different data structures have to be updated)
3. the locking requirements for each operation (and hence the achievable parallelism)
4. the speed (and success probability) of error recovery (because of the number of possible errors and their detectability/repairability)

## 4. Life Principles
### 4.1 There Ain't No Such Thing As A Free Lunch
Everything comes at a cost and involves costs, trade-offs, and compromises.

### 4.2 The Devil is Usually in the Details
- Enumerating (or even identifying) all of the interesting cases is a great deal of work.
- In many cases, it may not be obvious how to make a particular situation fit in to our beautiful high level structure.
- Sometimes we encounter one nasty case that simply refuses to fit into the new model.
- Sometimes we can extend our model to embrace the troublesome cases.
- Sometimes we can sub-case, preclude or exclude the troublesome cases.
- Sometimes we conclude that, while beautiful, our idea doesn't work.

### 4.3 Keep It Simple, Stupid and If it ain't broke, don't fix it
Complexity is the enemy. It is natural to look at a solution and recognize enhancements that would make it smarter, or more complete, or more elegant but they often prove to be counter-productive:
- Many optimizations require extra work to better handle special cases, and that extra work may greatly reduce the benefits of the optimization.
- Many ideas that seem simple at first become more complex as we examine all of the cases that have to be correctly handled.
- More complex solutions are likely to have more complex behavior, including unanticipated consequences. If we cannot predict the behavior associated with changes, we are likely to find that we have created new problems.
- More complex solutions are more difficult to understand, and hence more likely to have undetected errors and more difficult to maintain.

### 4.4 Be Clear About Your Goals
Having a clear understanding of our goals will enable us to resolve differences of opinion, make better decisions, and avoid distractions.

### 4.5 Responsibility and Sustainability
No software-managed resource can ever be lost. All memory and secondary storage will be recycled over and over again. Errors cannot be dismissed as unlikely events, and no error can be left un-handled. We are all responsible for anticipating and dealing with all the consequences of our actions. OS designers are not, by nature, optimists.


# Arpaci-Dusseau Chapter 1
**Key Ideas:** virtualization, concurrency, and persistence

# Arpaci-Dusseau Chapter 2: Introduction to Operating Systems
A running program executes instructions. The processor **fetches** an instruction from memory, **decodes** it (figures out what it is) and **executes** it. After it's done, the processor moves on to the next instruction. Keep in mind the program executes millions of instructions per second. This is the **Von Neuman** model of computing.

**Operating Systems** or OS are in charge of making sure the system operates correctly and efficiently in an easy-to-use manner. To do this, it uses a technique called **virtualization** in which the OS takes a **physical resource** (i.e. processor, memory, or disk) and transforms it into a more general powerful and easy-to use **virtual** form of itself which is why an OS is called a **virtual machine**.

In order to allow users to tell the OS what to do, it provides interfaces that the user can call (like APIs). A typical OS exports a few hundred **system calls** that are available to applications. This is why the OS provides a **standard library** to apps. 

The OS is also called **resource manager** (because of virtualization) as it allows:
- Many programs to run and concurrently access their own instructions and data (thus sharing memory),
- Many programs to access devices, thus sharing disks

Each of the CPU, memory, and disk is a **resource** of the OS so it is the OS's role to **manage** those resources efficiently or fairly. 

***Sample code that loops and prints:***
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <assert.h>
#include "common.h"

int 
main(int argc, char *argv[])
{
    if (argc != 2) {
    fprintf(stderr, "usage: cpu <string>\n");
    exit(1);
    }
    char *str = argv[1];
    while (1) {
        Spin(1);
        printf("%s\n", str);
    }
    return 0;
}
```

### 2.1 Virtualizing the CPU
Compiling the sample code above and running it on a single processor (or CPU) results in: 
```bash
prompt> gcc -o cpu cpu.c -Wall
prompt> ./cpu "A"
A
A
A
A
^C
prompt> 
```
Compling code but with diffrent instances of the same program results in:
```bash
prompt> ./cpu A & ./cpu B & ./cpu C & ./cpu D &
[1] 7353
[2] 7354
[3] 7355
[4] 7356
A
B
D
C
A
B
D
C
A
***
```
Somehow all the programs are running at the same time. This is because the OS gives an *illusion* that the system has a very large number of virtual CPUs. This is because turning a single CPU (or a small set of them) into an infinite number of CPUs allows many programs to run at once. This is called **virtualizing the CPU**.

### 2.2 Virtualizing the Memory
*Figures 2.3* shows a program that accesses Memory (mem.c):
```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include "common.h"

int 
main(int argc, char *argv[])
{
    int *p = malloc(sizeof(int)); // a1
    assert(p != NULL);
    printf("(%d) address pointed to by p: %p\n", getpid(), p); // a2
    *p = 0; // a3
    while (1){
        Spin(1);
        *p = *p + 1;
        printf("(%d) p: %d\n", getpid(), *p); // a4
    }
}
```
**CODE EXPLANATION:** *Figure 2.3* allocates some memory by calling ``` malloc()```. The program does a couple of things. First, it allocates some memory (line a1). Then, it prints out the address of the memory (a2), and then puts the number zero into the first slot of the newly allocated memory (a3). Finally, it loops, delaying for a second and incrementing the value stored at the address held in p. With every print statement, it also prints out what is called the process identifier (the PID) of the running program.
 his PID is unique per running process.

---

Memory is just an array of bytes; to **read** memory, one must specify an **address** to be able to access the data stored there; to **write** (or **update**) memory, one must also specify the data to be written to the given address. A program keeps all of its data structures in memory, and accesses them through various instructions, like loads and stores or other explicit instructions that access memory in doing their work. 


*Figure 2.4*: Running the Memory Program multiple times:
```bash
prompt> ./mem
(2134) address pointed to by p: 0x200000
(2134) p: 1
(2134) p: 2
(2134) p: 3
(2134) p: 4
(2134) p: 5
ˆC

prompt> ./mem & ./mem &
[1] 24113
[2] 24114
(24113) address pointed to by p: 0x200000
(24114) address pointed to by p: 0x200000
(24113) p: 1
(24114) p: 1
(24114) p: 2
(24113) p: 2
(24113) p: 3
(24114) p: 3
(24113) p: 4
(24114) p: 4
***
```

What happens here is **virtualization** as each process accesses its own private **virtual address space** or **address space**, which the OS somehows maps onto the physical memory of the machine. A memory reference within one running program doesn't affect the address space of other processes as the program has physical memory to itself. This is because physical memory is a shared resource. 

### 2.3 Concurrency
*Figure 2.5:* A Multi-thread Program (threads.c)
```c
#include <stdio.h>
#include <stdlib.h>
#include "common.h"
#include "common_threads.h"

volatile int counter = 0;
int loops;

void *worker(void *arg) {
    int i;
    for (i = 0; i < loops; i++) {
    counter++;
    }
    return NULL;
}

int main(int argc, char *argv[]) {
    if (argc != 2) {
    fprintf(stderr, "usage: threads <value>\n");
    exit(1);
    }
    loops = atoi(argv[1]);
    pthread_t p1, p2;
    printf("Initial value : %d\n", counter);

    Pthread_create(&p1, NULL, worker, NULL);
    Pthread_create(&p2, NULL, worker, NULL);
    Pthread_join(p1, NULL);
    Pthread_join(p2, NULL);
    printf("Final value : %d\n", counter);
    return 0;
}
```
The figure above uses ```Pthread.create()``` and creates two **threads**. (Think of a thread as a function running with the same memory space as other functions, with more than one of them active at a time). Each thread starts running in a routine called ```worker()``` which increases counter in a loop for loops number of times.

Concurrency is used to refer to a host of problems that arise and must be addressed when working on many things at once in the same program.


```bash
prompt> gcc -o threads threads.c -Wall -pthread
prompt> ./threads 1000
Initial value: 0
Final value: 2000
```

```bash
prompt> ./threads 100000
Initial value : 0
Final value : 143012 // huh??
prompt> ./threads 100000
Initial value : 0
Final value : 137298 // what the??
```