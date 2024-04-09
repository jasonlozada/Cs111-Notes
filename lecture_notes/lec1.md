# Lecture 1: Introduction to Operating Systems
## Background
RAM reads 32 bits writes 64 bits

- cpu instructions examples: moves, add, jmp, or sqrtpd
- buzz: read/request sense
- USB: read or write a block of data
- mouse reports X and Y (or Z depending on how avdvance the computer is) movements
- PC is used to write and move pixels

## Operating Systems
####  Why we need it
- Helping software run at a higher level to do things it wants to do
    - peroforms complex operations that interact, using various hardware, and probably various bits of software
- OS does it job while hiding __complexity__ and making sure nothing gets in the way


#### Definition
- **System software** (a piece of software meant help to run the system as a whole) intended to provide support for higher level appslications
    - Ex: higher level system software applications
    - Used mainly for user processes
- OS is a software that sits between the hardware and everything else
- The software that higes nasty details from the hardware, software, or common tasks
    - In charge of the hardware

#### Why we care about OS?
- Every computer device you will ever use has an OS
    - Ex: servers, latops, desktop machines, tables, smart phones, game consoles, set-top boxes, washing machines, eletric cars (i.e.Tesla), "smart" objects
- Many hard problems have been tackled in the context of OS
    - How to coordinate separate computations
    - How to manage shared resources 
    – How to virtualize hardware and software
    – How to organize communications
    – How to protect your computing resources
- The operating system solutions are often
applicable to programs and systems you write

#### How do you work with OSes?
- Configure them
- Use their features when writing programs
- Reliance on services such as:
    1. Memory Management
    2. Persistent storage
    3. Scheduling and synchronization
        - Communication between different processes (i.e. Zoom running on background while I'm on Google Chrome)
    4. Interprocess communications
    5. Security
- *Note that these services are ran around 50-100 times at once.

#### OS and Object Correlations
- OS views services as objects and operations
    - Behing every objext there is a data structure

- Interface vs implementation
    - Interface is just doing the tasks that is assigned
    - An immplementation is not a specification
    - Many compliant implementations are possible
    