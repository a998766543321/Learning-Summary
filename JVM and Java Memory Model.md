## Background

Java has **2** such environments and everyone working with Java has to start their work after setting up one of these environments on their local development or production environment platforms

* JRE (Java Runtime Environment): the minimum environment needed for running a Java application (no support for developing). It includes **JVM (Java Virtual Machine)** and deployment tools. >> for everyone 
* JDK (Java Development Kit): the complete development environment used for developing and executing Java applications. It includes both JRE and development tools. >> for developers


Java source codes are compiled into an intermediate state called **bytecode** (i.e. a **.class** file) using the **Java Compiler (javac)** which comes inbuilt with **JDK**.

This bytecode is in hexadecimal format with **opcode-operand** lines and **JVM** can interpret these instructions (without further recompilations) into native machine language

Therefore, bytecode acts as a **platform-independent intermediary state** which is portable among any JVM regardless of underlying OS and hardware architecture

However, since JVMs are developed to run and communicate with the underlying hardware & OS structure, we need to select the appropriate JVM version for our OS version (Windows, Linux, Mac) and processor architecture (x86, x64).

## JVM Architecture

* JVM is only a specification, and its implementation is different from vendor to vendor
* JVM resides on the RAM
* JVM is multi-threaded

## JVM components
1. Class Loader Subsystem
    * It loads, links, and initializes the class file (.class) when it refers to a class for the first time at runtime **(not compile time)**.
    * It generates corresponding binary data and save the following information in the Method area for each class separately
    
2. Runtime Data Area
    * the memory areas assigned when the JVM program runs on the OS
    * Method Area
      * Shared among Threads
      * stores class level data (including static variables)
    * Heap Area
      * Shared among Threads
      * stores Information of all objects and their corresponding instance variables and arrays
    * Stack Area
      * Per thread
      * store method calls + its ordering
    * PC Registers
      * Per thread
      * hold the address of currently-executing instruction (memory address in the Method area)
    * Native Method Stack
      * Per thread
      * a direct mapping between a Java thread and a native operating system thread
      
3. Execution Engine
    * Interpreter
      * interprets the bytecode and executes the instructions one-by-one
      * bytecode execution is slower than native code (machine code)
    * Just-In-Time (JIT) Compiler 
      * compiles the entire bytecode to native code (machine code) then store in the cache  <-- expensive process
      * used in Hotspot (instances where one method is called multiple times) for maximize the value
    * Garbage Collector (GC)
      * Once an object is no longer referenced and therefore is not reachable by the application code, the garbage collector removes it and reclaims the unused memory 
      
5. Java Native Interface (JNI)

6. Native Method Libraries
  
## JVM Summary
* Java is considered as both interpreted and compiled Language.
* By design, Java is slow due to dynamic linking and run-time interpreting.
* JIT compiler compensate for the disadvantages of the interpreter for repeating operations by keeping a native code instead of bytecode.
* The latest Java versions address performance bottlenecks in its original architecture.
* JVM is only a specification. Vendors are free to customize, innovate, and improve its performance during the implementation.


![JVM png](/images/JVM.png?raw=true "Test")

Reference:
[Understanding JVM Architecture](https://medium.com/platform-engineer/understanding-jvm-architecture-22c0ddf09722)


## JVM Memory Model
   1. Heap Memory
      ![Heap Memory.PNG](/images/Heap Memory.PNG?raw=true "Test")

      * Young Generation
         * This is reserved for containing newly-allocated objects
         * Young Gen includes three parts — Eden Memory and two Survivor Memory spaces (S0, S1)
         * Most of the newly-created objects goes Eden space.
         * When Eden space is filled with objects, Minor GC (a.k.a. Young Collection) is performed and all the survivor objects are moved to one of the survivor spaces.
         * Minor GC also checks the survivor objects and move them to the other survivor space. So at a time, one of the survivor space is always empty.
         * Objects that are survived after many cycles of GC, are moved to the Old generation memory space. Usually it’s done by setting a threshold for the age of the young generation objects before they become eligible to promote to Old generation.

      * Old Generation
         * This is reserved for containing long lived objects that could survive after many rounds of Minor GC
         * When Old Gen space is full, Major GC (a.k.a. Old Collection) is performed (usually takes longer time)

   2. Non-Heap Memory
   3. Cache Memory
   4. Stack Memory
